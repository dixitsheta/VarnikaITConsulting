---
title: "Building Custom SAC Widgets: A Developer's Complete Guide"
date: 2024-11-15T14:00:00+00:00
draft: false
author: "Varnika IT Consulting"
description: "Learn how to create custom widgets for SAP Analytics Cloud using JavaScript, HTML5, and CSS3. Includes code examples, best practices, and deployment guide."
categories: ["SAP Analytics Cloud", "Custom Development"]
tags: ["SAC", "Custom Widgets", "JavaScript", "D3.js", "SDK", "Development"]
---

## Introduction

While SAP Analytics Cloud (SAC) offers 25+ standard chart types, sometimes your business needs require unique visualizations or interactive components that don't exist out of the box. That's where **custom widgets** come in.

This comprehensive guide walks you through building custom SAC widgets from scratch, covering:
- Widget SDK fundamentals
- Development environment setup
- Code structure and best practices
- Deployment and versioning
- Real-world examples with code

Whether you're building a specialized chart, integrating third-party APIs, or creating branded UI components, this guide has you covered.

## When to Build Custom Widgets

### ✓ Good Use Cases

**1. Specialized Visualizations**
- Sankey diagrams for flow analysis
- Network graphs for relationship mapping
- Custom gauge charts matching brand guidelines
- 3D visualizations for spatial data

**2. Third-Party Integrations**
- Embed Google Maps with custom markers
- Real-time stock tickers from financial APIs
- Social media sentiment feeds
- IoT sensor dashboards

**3. Advanced Interactivity**
- Drag-and-drop planning interfaces
- Custom drill-down animations
- Gamification elements (leaderboards, progress rings)
- Interactive tutorials within dashboards

**4. Branding Requirements**
- Corporate-specific KPI cards
- Branded loading animations
- Custom icon sets
- Proprietary chart types

### ❌ When NOT to Build Custom Widgets

- Standard chart types exist that meet 90% of requirements
- Timeline is critical (development takes 2-4 weeks)
- No in-house JavaScript expertise
- Widget would be used in only 1-2 dashboards

**Alternative:** Consider SAC's native customization options first:
- Custom styling with CSS
- Scripting for dynamic behavior
- Combined standard charts with overlay techniques

## Prerequisites

### Technical Skills Required

| Skill | Level | Purpose |
|-------|-------|---------|
| JavaScript (ES6+) | Intermediate | Widget logic and interactivity |
| HTML5/CSS3 | Intermediate | Widget structure and styling |
| D3.js or Chart.js | Basic-Intermediate | Data visualization libraries |
| JSON | Basic | Data handling and configuration |
| Git | Basic | Version control |
| SAC Platform | Intermediate | Dashboard integration |

### Development Tools

**Required:**
- Text editor (VS Code recommended)
- SAC Custom Widget SDK
- Node.js v14+ (for local development server)
- Chrome/Firefox DevTools

**Optional:**
- Postman (API testing)
- GitHub/GitLab (version control)
- Figma (design mockups)

## Widget SDK Architecture

### File Structure

```
my-custom-widget/
├── widget.json           # Widget metadata and properties
├── main.js              # Main widget logic
├── styling.css          # Widget styles
├── icon.png             # Widget icon (48x48px)
├── /libs/               # Third-party libraries
│   ├── d3.min.js
│   └── chart.min.js
└── README.md            # Documentation
```

### widget.json - Configuration File

```json
{
  "id": "com.varnika.sankey_chart",
  "version": "1.0.0",
  "name": "Sankey Flow Diagram",
  "description": "Visualize flow and relationships between entities",
  "vendor": "Varnika IT Consulting",
  "license": "Apache 2.0",
  "icon": "icon.png",
  "webcomponents": [
    {
      "kind": "main",
      "tag": "sankey-widget",
      "url": "main.js",
      "integrity": "",
      "ignoreIntegrity": true
    }
  ],
  "properties": {
    "width": {
      "type": "integer",
      "default": 800
    },
    "height": {
      "type": "integer",
      "default": 600
    },
    "colorScheme": {
      "type": "string",
      "description": "Color palette for flows",
      "default": "blues",
      "enum": ["blues", "greens", "category10"]
    },
    "nodeWidth": {
      "type": "integer",
      "description": "Width of node rectangles",
      "default": 15,
      "minimum": 10,
      "maximum": 30
    }
  },
  "methods": {
    "setData": {
      "description": "Update widget data",
      "parameters": [
        {
          "name": "data",
          "type": "object",
          "description": "Nodes and links array"
        }
      ]
    }
  },
  "events": {
    "onNodeClick": {
      "description": "Triggered when user clicks a node"
    }
  }
}
```

## Example 1: Building a Sankey Diagram Widget

### Step 1: Define the HTML Structure

**main.js - Part 1: Template**

```javascript
(function() {
  let template = document.createElement("template");
  template.innerHTML = `
    <style>
      :host {
        display: block;
        font-family: "72", Arial, sans-serif;
      }
      
      #sankeyContainer {
        width: 100%;
        height: 100%;
        position: relative;
      }
      
      .node rect {
        fill-opacity: 0.9;
        stroke: #fff;
        stroke-width: 2px;
        cursor: pointer;
        transition: fill-opacity 0.2s;
      }
      
      .node rect:hover {
        fill-opacity: 1;
      }
      
      .node text {
        font-size: 12px;
        pointer-events: none;
        fill: #333;
      }
      
      .link {
        fill: none;
        stroke-opacity: 0.3;
        transition: stroke-opacity 0.2s;
      }
      
      .link:hover {
        stroke-opacity: 0.6;
      }
      
      .tooltip {
        position: absolute;
        background: rgba(0, 0, 0, 0.8);
        color: white;
        padding: 8px 12px;
        border-radius: 4px;
        font-size: 12px;
        pointer-events: none;
        opacity: 0;
        transition: opacity 0.2s;
      }
    </style>
    
    <div id="sankeyContainer">
      <svg id="sankeyChart"></svg>
      <div class="tooltip"></div>
    </div>
  `;

  class SankeyWidget extends HTMLElement {
    constructor() {
      super();
      this.attachShadow({ mode: "open" });
      this.shadowRoot.appendChild(template.content.cloneNode(true));
      
      this._props = {
        width: 800,
        height: 600,
        colorScheme: "blues",
        nodeWidth: 15
      };
      
      this._data = null;
    }

    connectedCallback() {
      this._render();
    }

    // Property setters
    set width(value) {
      this._props.width = value;
      this._render();
    }

    set height(value) {
      this._props.height = value;
      this._render();
    }

    set colorScheme(value) {
      this._props.colorScheme = value;
      this._render();
    }

    // Data setter
    setData(data) {
      this._data = data;
      this._render();
    }

    _render() {
      if (!this._data) return;

      const container = this.shadowRoot.getElementById("sankeyContainer");
      const svg = this.shadowRoot.getElementById("sankeyChart");
      const tooltip = this.shadowRoot.querySelector(".tooltip");

      // Clear previous chart
      svg.innerHTML = "";

      // Set SVG dimensions
      svg.setAttribute("width", this._props.width);
      svg.setAttribute("height", this._props.height);

      // Load D3.js if not already loaded
      this._loadD3().then(() => {
        this._drawSankey(svg, tooltip);
      });
    }

    _loadD3() {
      return new Promise((resolve) => {
        if (window.d3) {
          resolve();
        } else {
          const script = document.createElement("script");
          script.src = "https://d3js.org/d3.v7.min.js";
          script.onload = () => resolve();
          document.head.appendChild(script);
        }
      });
    }

    _drawSankey(svg, tooltip) {
      const width = this._props.width;
      const height = this._props.height;
      const nodeWidth = this._props.nodeWidth;
      
      // Process data
      const nodes = this._data.nodes.map(d => ({ ...d }));
      const links = this._data.links.map(d => ({ ...d }));

      // Create Sankey generator
      const sankey = d3.sankey()
        .nodeWidth(nodeWidth)
        .nodePadding(10)
        .extent([[1, 1], [width - 1, height - 6]]);

      const { nodes: sankeyNodes, links: sankeyLinks } = sankey({
        nodes: nodes,
        links: links
      });

      // Color scale
      const color = this._getColorScale(this._props.colorScheme);

      // Create SVG groups
      const g = d3.select(svg).append("g");

      // Draw links
      g.append("g")
        .selectAll("path")
        .data(sankeyLinks)
        .join("path")
        .attr("class", "link")
        .attr("d", d3.sankeyLinkHorizontal())
        .attr("stroke", d => color(d.source.name))
        .attr("stroke-width", d => Math.max(1, d.width))
        .on("mouseover", (event, d) => {
          tooltip.style.opacity = 1;
          tooltip.innerHTML = `
            ${d.source.name} → ${d.target.name}<br/>
            <strong>Value: ${d.value.toLocaleString()}</strong>
          `;
        })
        .on("mousemove", (event) => {
          tooltip.style.left = (event.pageX + 10) + "px";
          tooltip.style.top = (event.pageY - 20) + "px";
        })
        .on("mouseout", () => {
          tooltip.style.opacity = 0;
        });

      // Draw nodes
      const node = g.append("g")
        .selectAll("g")
        .data(sankeyNodes)
        .join("g")
        .attr("class", "node");

      node.append("rect")
        .attr("x", d => d.x0)
        .attr("y", d => d.y0)
        .attr("height", d => d.y1 - d.y0)
        .attr("width", d => d.x1 - d.x0)
        .attr("fill", d => color(d.name))
        .on("click", (event, d) => {
          // Dispatch custom event
          this.dispatchEvent(new CustomEvent("onNodeClick", {
            detail: { node: d.name, value: d.value }
          }));
        });

      node.append("text")
        .attr("x", d => d.x0 < width / 2 ? d.x1 + 6 : d.x0 - 6)
        .attr("y", d => (d.y1 + d.y0) / 2)
        .attr("dy", "0.35em")
        .attr("text-anchor", d => d.x0 < width / 2 ? "start" : "end")
        .text(d => d.name);
    }

    _getColorScale(scheme) {
      const schemes = {
        blues: d3.schemeBlues[9],
        greens: d3.schemeGreens[9],
        category10: d3.schemeCategory10
      };
      
      return d3.scaleOrdinal(schemes[scheme] || schemes.blues);
    }
  }

  customElements.define("sankey-widget", SankeyWidget);
})();
```

### Step 2: Prepare Sample Data

**Example dataset for testing:**

```json
{
  "nodes": [
    { "name": "Web Traffic" },
    { "name": "Email Campaign" },
    { "name": "Social Media" },
    { "name": "Product Page" },
    { "name": "Cart" },
    { "name": "Checkout" },
    { "name": "Purchase" },
    { "name": "Abandoned" }
  ],
  "links": [
    { "source": 0, "target": 3, "value": 5000 },
    { "source": 1, "target": 3, "value": 2000 },
    { "source": 2, "target": 3, "value": 1500 },
    { "source": 3, "target": 4, "value": 6000 },
    { "source": 3, "target": 7, "value": 2500 },
    { "source": 4, "target": 5, "value": 4500 },
    { "source": 4, "target": 7, "value": 1500 },
    { "source": 5, "target": 6, "value": 3500 },
    { "source": 5, "target": 7, "value": 1000 }
  ]
}
```

### Step 3: Deploy to SAC

**Deployment Options:**

**Option A: Direct Upload**
1. Open SAC → Files → Public → Custom Widgets
2. Click "Upload" → Select widget files
3. Widget appears in chart picker

**Option B: CDN Hosting**
1. Host widget files on public CDN
2. Reference URL in SAC widget configuration
3. Benefit: Versioning and central updates

**Option C: SAP BTP Deployment**
```bash
# Package widget
zip -r sankey-widget.zip widget.json main.js icon.png

# Deploy via SAP BTP CLI
cf deploy sankey-widget.zip -t varnika-sac-widgets
```

## Example 2: KPI Card with Sparkline

**Simpler widget - perfect for beginners:**

```javascript
(function() {
  let template = document.createElement("template");
  template.innerHTML = `
    <style>
      :host {
        display: block;
      }
      
      .kpi-card {
        background: linear-gradient(135deg, #003366 0%, #004080 100%);
        color: white;
        padding: 24px;
        border-radius: 12px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        font-family: "72", Arial, sans-serif;
      }
      
      .kpi-label {
        font-size: 14px;
        opacity: 0.9;
        margin-bottom: 8px;
      }
      
      .kpi-value {
        font-size: 36px;
        font-weight: bold;
        margin-bottom: 4px;
      }
      
      .kpi-change {
        font-size: 16px;
        margin-bottom: 12px;
      }
      
      .kpi-change.positive {
        color: #4CAF50;
      }
      
      .kpi-change.negative {
        color: #FF5252;
      }
      
      .sparkline {
        width: 100%;
        height: 40px;
      }
    </style>
    
    <div class="kpi-card">
      <div class="kpi-label"></div>
      <div class="kpi-value"></div>
      <div class="kpi-change"></div>
      <canvas class="sparkline"></canvas>
    </div>
  `;

  class KPICard extends HTMLElement {
    constructor() {
      super();
      this.attachShadow({ mode: "open" });
      this.shadowRoot.appendChild(template.content.cloneNode(true));
      
      this._label = "";
      this._value = 0;
      this._change = 0;
      this._history = [];
    }

    set label(val) {
      this._label = val;
      this.shadowRoot.querySelector(".kpi-label").textContent = val;
    }

    set value(val) {
      this._value = val;
      this.shadowRoot.querySelector(".kpi-value").textContent = 
        this._formatNumber(val);
    }

    set change(val) {
      this._change = val;
      const elem = this.shadowRoot.querySelector(".kpi-change");
      const arrow = val >= 0 ? "▲" : "▼";
      elem.textContent = `${arrow} ${Math.abs(val).toFixed(1)}%`;
      elem.className = `kpi-change ${val >= 0 ? "positive" : "negative"}`;
    }

    set history(data) {
      this._history = data;
      this._drawSparkline();
    }

    _formatNumber(num) {
      if (num >= 1000000) {
        return (num / 1000000).toFixed(1) + "M";
      } else if (num >= 1000) {
        return (num / 1000).toFixed(1) + "K";
      }
      return num.toLocaleString();
    }

    _drawSparkline() {
      const canvas = this.shadowRoot.querySelector(".sparkline");
      const ctx = canvas.getContext("2d");
      const width = canvas.width = canvas.offsetWidth * 2; // Retina
      const height = canvas.height = canvas.offsetHeight * 2;
      
      ctx.scale(2, 2);
      
      const data = this._history;
      if (!data || data.length < 2) return;

      const max = Math.max(...data);
      const min = Math.min(...data);
      const range = max - min;
      
      const xStep = width / 2 / (data.length - 1);
      const yScale = (height / 2 - 10) / range;

      ctx.beginPath();
      ctx.strokeStyle = "#F4C430"; // Gold
      ctx.lineWidth = 2;
      
      data.forEach((val, i) => {
        const x = i * xStep;
        const y = height / 2 - 5 - ((val - min) * yScale);
        
        if (i === 0) {
          ctx.moveTo(x, y);
        } else {
          ctx.lineTo(x, y);
        }
      });
      
      ctx.stroke();
    }
  }

  customElements.define("kpi-card", KPICard);
})();
```

**Usage in SAC:**

```javascript
// In SAC story script
var kpiWidget = Canvas.getWidgetById("KPI_1");

kpiWidget.label = "Monthly Revenue";
kpiWidget.value = 1250000;
kpiWidget.change = 15.3; // +15.3%
kpiWidget.history = [980000, 1050000, 1100000, 1150000, 1250000];
```

## Best Practices

### 1. Performance Optimization

**✓ Do:**
- Debounce resize events (300ms delay)
- Use `requestAnimationFrame` for animations
- Cache DOM queries
- Lazy load large libraries
- Implement virtual scrolling for large datasets

**❌ Don't:**
- Manipulate DOM on every data point
- Load full D3.js for simple charts (use Chart.js or native Canvas)
- Create memory leaks with event listeners
- Block main thread with heavy calculations

### 2. Error Handling

```javascript
setData(data) {
  try {
    // Validate data structure
    if (!data || !data.nodes || !data.links) {
      throw new Error("Invalid data format");
    }
    
    // Validate data integrity
    const maxIndex = data.nodes.length - 1;
    data.links.forEach(link => {
      if (link.source > maxIndex || link.target > maxIndex) {
        throw new Error("Link references non-existent node");
      }
    });
    
    this._data = data;
    this._render();
    
  } catch (error) {
    console.error("Widget data error:", error);
    this._showError(error.message);
  }
}

_showError(message) {
  const container = this.shadowRoot.getElementById("sankeyContainer");
  container.innerHTML = `
    <div style="color: red; padding: 20px;">
      <strong>Error:</strong> ${message}
    </div>
  `;
}
```

### 3. Accessibility

```html
<!-- Add ARIA attributes -->
<div role="img" aria-label="Sankey diagram showing flow from sources to destinations">
  <svg>
    <g role="graphics-symbol" aria-label="Node: Web Traffic, Value: 5000">
      <rect></rect>
      <text>Web Traffic</text>
    </g>
  </svg>
</div>
```

### 4. Responsive Design

```javascript
connectedCallback() {
  // Observe size changes
  this._resizeObserver = new ResizeObserver(entries => {
    clearTimeout(this._resizeTimeout);
    this._resizeTimeout = setTimeout(() => {
      const width = entries[0].contentRect.width;
      const height = entries[0].contentRect.height;
      this.width = width;
      this.height = height;
    }, 300); // Debounce
  });
  
  this._resizeObserver.observe(this);
}

disconnectedCallback() {
  this._resizeObserver.disconnect();
}
```

## Testing Your Widget

### Local Testing Setup

**1. Create test HTML file:**

```html
<!DOCTYPE html>
<html>
<head>
  <title>Widget Test</title>
  <script src="main.js"></script>
</head>
<body>
  <h1>Sankey Widget Test</h1>
  
  <sankey-widget 
    id="testWidget" 
    style="width: 800px; height: 600px; display: block;">
  </sankey-widget>
  
  <script>
    const widget = document.getElementById("testWidget");
    
    // Test data
    widget.setData({
      nodes: [
        { name: "A" }, { name: "B" }, { name: "C" },
        { name: "D" }, { name: "E" }
      ],
      links: [
        { source: 0, target: 2, value: 100 },
        { source: 1, target: 2, value: 80 },
        { source: 2, target: 3, value: 120 },
        { source: 2, target: 4, value: 60 }
      ]
    });
    
    // Test property changes
    widget.colorScheme = "greens";
    widget.nodeWidth = 20;
    
    // Test events
    widget.addEventListener("onNodeClick", (e) => {
      console.log("Node clicked:", e.detail);
    });
  </script>
</body>
</html>
```

**2. Run local server:**

```bash
# Using Python
python3 -m http.server 8000

# Using Node.js
npx http-server

# Visit: http://localhost:8000/test.html
```

### Unit Testing

```javascript
// test/widget.test.js
describe("SankeyWidget", () => {
  let widget;
  
  beforeEach(() => {
    widget = document.createElement("sankey-widget");
    document.body.appendChild(widget);
  });
  
  afterEach(() => {
    document.body.removeChild(widget);
  });
  
  it("should render with valid data", () => {
    widget.setData({
      nodes: [{ name: "A" }, { name: "B" }],
      links: [{ source: 0, target: 1, value: 10 }]
    });
    
    const svg = widget.shadowRoot.querySelector("svg");
    expect(svg).toBeTruthy();
  });
  
  it("should handle invalid data gracefully", () => {
    widget.setData(null);
    // Should not throw error
    expect(widget.shadowRoot.querySelector(".error")).toBeFalsy();
  });
});
```

## Deployment Checklist

```
✓ Code Quality
  □ No console.log() statements in production
  □ All errors caught and handled
  □ Code minified for production
  □ No hardcoded test data
  
✓ Performance
  □ Loads in < 1 second
  □ Smooth animations (60fps)
  □ Works with 10,000+ data points
  □ No memory leaks after 100 renders
  
✓ Compatibility
  □ Tested in Chrome, Firefox, Safari
  □ Works on mobile devices
  □ Compatible with SAC's latest version
  □ No conflicts with other custom widgets
  
✓ Documentation
  □ widget.json complete and accurate
  □ README with usage examples
  □ API documentation for all properties/methods
  □ Sample data provided
  
✓ Versioning
  □ Version number updated
  □ Changelog maintained
  □ Breaking changes documented
```

## Conclusion

Custom SAC widgets unlock unlimited possibilities for visualizations and interactivity. While they require JavaScript development skills, the investment pays off when you need specialized analytics experiences that drive better business decisions.

**Key Takeaways:**
- Start with simple widgets (KPI cards) before complex visualizations
- Leverage proven libraries (D3.js, Chart.js) rather than building from scratch
- Test thoroughly across browsers and devices
- Document properties, methods, and events clearly
- Version control your widgets and maintain changelogs

---

## Need Custom SAC Widget Development?

Varnika IT Consulting has built 50+ custom widgets for SAC, from advanced visualizations to third-party API integrations. Our team combines SAP expertise with modern web development skills to create widgets that enhance your analytics platform.

**Pricing:** $2,000 - $15,000 per widget depending on complexity

**[Start Your Custom Widget Project →](/contact/)**

---

*Published: November 15, 2024 | Reading Time: 14 minutes*
