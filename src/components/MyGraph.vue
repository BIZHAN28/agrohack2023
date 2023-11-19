<template>
  <div>
    <div id="cy"></div>
  </div>
</template>

<script>
import cytoscape from 'cytoscape';

function generateColor(name) {
  const hashCode = name.split('').reduce((acc, char) => char.charCodeAt(0) + (acc << 6) + (acc << 16) - acc, 0);
  const color = (hashCode & 0x00FFFFFF).toString(16).toUpperCase();
  return `#${'00000'.substring(0, 6 - color.length)}${color}`;
}

export default {
  mounted() {
    this.fetchGraphData();
  },
  methods: {
    async fetchGraphData() {
      try {
        const response = await import('./data.json'); // Replace with the path to your JSON file
        const data = response.default;

        this.initGraph(data);
      } catch (error) {
        console.error('Error fetching or parsing JSON data:', error);
      }
    },
    initGraph(data) {
      cytoscape({
        container: document.getElementById('cy'),
        elements: data,
        layout: {          name: 'cose',
          idealEdgeLength: 100,
          nodeOverlap: 20,
          refresh: 20,
          fit: true,
          padding: 30,
          randomize: false,
          componentSpacing: 100,
          nodeRepulsion: function () {
            return 2048;
          },
          edgeElasticity: function () {
            return 32;
          },
          nestingFactor: 8,
          gravity: 0.25,
          numIter: 1000,
          initialTemp: 200,
          coolingFactor: 0.95,
          minTemp: 1.0,
        },
        style: [
          {
            selector: 'node',
            style: {
              'content': 'data(label)', // Display the label property of the node
              'text-valign': 'center',
              'color': 'white',
              'text-outline-width': 2,
              'text-outline-color': '#888',
            },
          },
          {
            selector: 'edge',
            style: {
              'width': 2,
              'line-color': '#ccc',
              'curve-style': 'bezier',
            },
          },
          {
            selector: 'node[name="man"]',
            style:{
              'background-color': (ele) => generateColor(ele.data('name')),
            }
          }
        ],
      });
    },
  },
};
</script>

<style>
#cy {
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 999;
}
</style>
