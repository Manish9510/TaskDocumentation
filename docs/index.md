# Welcome to MkDocs

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    Documentation: Parsing and Visualizing Switch Data

•	Overview
This document describes the process used to parse, transform, and visualize switch data from a file. The goal was to extract switch and node information, structure it hierarchically, and represent it in a tree format using D3.js. 

•	Steps and File Descriptions

 1. Parsing the Switch Data
Purpose: Convert raw switch data from a information file into a structured JSON format.

 File: `parse_switch_data.js`

Description: This script reads the binary file containing switch data, parses it, and writes the output to a JSON file. The parsing involves extracting switch names, GUIDs, descriptions, and nodes. It also identifies whether a switch has links to other switches.

-Input: A binary file (`iblatest.unknown`).
- Output: A JSON file (`parsed_data_new_yukti.json`) containing an array of switch objects with detailed information.

Key Functions:
1. **`parseSwitchData(filePath)`**: Parses the input file to extract switch details and node ranges.
2. **`expandNodeRanges(nodes)`**: Expands node ranges into individual nodes.
3. **`processRange(range, expandedNodes)`**: Processes node ranges to generate a list of nodes.

**Output JSON Structure**:
```json
[
    {
        "switch_name": "ibsw1",
        "guid": "0x0800380300b90060",
        "description": "pm2-wpe1",
        "nodes": [
            "Mellanox",
            "cn108",
            "cn109",
            // more nodes
        ],
        "hasLink": true
    },
    // more switches
]
```

2. Transforming the Data
Purpose: Structure the parsed data into a format suitable for visualization. It categorizes switches into L1 and L2 and prepares a hierarchical format.

 File: `transform_data.js`

Description: This script processes the parsed JSON data to categorize switches and organize them into a tree structure. L1 switches have links and contain nodes, while L2 switches do not.

- Input: The JSON file from the previous step (`parsed_data_new_yukti.json`).
- Output: A JSON file (`result_new_yukti.json`) containing the structured data for visualization.

Key Functions:
1. Categorization: Distinguishes between L1 and L2 switches based on their link status.
2. Hierarchical Structure: Links L0 switches (nodes) to L1 switches based on their presence in the node list.

Output JSON Structure:
```json
{
    "l2Switches": [
        // L2 switches
    ],
    "l1Switches": [
        {
            "switch_name": "ibsw1",
            "nodes": [
                "Mellanox",
                "cn108",
                // more nodes
            ]
        },
        // more L1 switches
    ]
}
```

 	3. Visualizing the Data
Purpose: Render the structured data into a visual tree representation using D3.js.

 File: `index.html`

Description: The HTML file sets up the basic structure for displaying the visualization. It includes references to the D3.js library and the JavaScript file that creates the visualization.

Key Elements:
- `<div id="tree-container"></div>`: Container for the tree visualization.
- Script Tags: Include D3.js and the custom script for visualization.

 File: `script.js`

Description: This script uses D3.js to create and render the tree structure based on the JSON data. It creates SVG elements for nodes, links, and labels.

Key Functions:
1. Data Loading: Loads the JSON data using `d3.json()`.
2. Tree Layout: Uses `d3.tree()` to compute the tree layout.
3. SVG Elements: Appends SVG elements to represent nodes, links, and labels.

Output:
- Visualization: A tree diagram where nodes represent switches and links represent connections.

File: `styles.css`

Description: The CSS file styles the tree visualization, including node colors, link styles, and label positions.

Key CSS Classes:
- **`.link`**: Styles for the lines connecting nodes.
- **`.node`**: Styles for the nodes in the tree.
- **`.label`**: Styles for the text labels on the nodes.

Example CSS:
```css
#tree-container {
  margin: 20px;
}

.link {
  stroke: #ccc;
  stroke-width: 2px;
}

.node {
  fill: #000;
}

.label {
  font-size: 12px;
  fill: #333;
}
```

 Conclusion

By following these steps, you can effectively parse, transform, and visualize switch data. The provided scripts and files handle the entire process from raw data extraction to a visual tree representation, making it easy to understand and analyze the switch network.

