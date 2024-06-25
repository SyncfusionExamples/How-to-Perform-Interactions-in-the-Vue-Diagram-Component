<template>
  <div class="container">
    <ejs-button class="button" @click="onSelect">Select</ejs-button>
    <ejs-button class="button" @click="onClearSelection">Clear Selection</ejs-button>
  </div>
  <ejs-diagram ref="diagramObject" :height="height" :width="width" :nodes="nodes" :connectors="connectors" />
</template>

<script>

let diagramInstance;

/* const nodes = [
  {
    id: 'node1', offsetX: 300, offsetY: 200,
    width: 200, height: 200,
    style: { fill: 'lightblue', strokeColor: 'lightblue' },
    annotations: [{ content: 'Node 1' }],
    constraints: NodeConstraints.Default & ~NodeConstraints.Select
  },
  {
    id: 'node2', offsetX: 800, offsetY: 200,
    width: 200, height: 200,
    style: { fill: 'lightblue', strokeColor: 'lightblue' },
    annotations: [{ content: 'Node 2' }],
    constraints: NodeConstraints.Default & ~NodeConstraints.Delete
  }];

const connectors = [
  {
    id: "connector1",
    sourceID: "node1",
    targetID: "node2",
    type: 'Straight',
    segments: [{ type: 'Straight' }],
    annotations: [{ content: 'Straight Connector', style: { fill: 'white' } }],
    constraints: ConnectorConstraints.Default & ~ConnectorConstraints.Delete
  }
]; */

let nodes = [
  {
    id: "startNode",
    offsetX: 440,
    offsetY: 60,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Terminator" },
    annotations: [{ content: 'Start' }]
  },
  {
    id: "inputNode",
    offsetX: 440,
    offsetY: 200,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Data" },
    annotations: [{ content: 'Enter a number', }]
  },
  {
    id: "decisionNode",
    offsetX: 440,
    offsetY: 340,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Decision" },
    annotations: [{ content: 'N divisible by 2 ?' }]
  },
  {
    id: "processEvenNode",
    offsetX: 700,
    offsetY: 340,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Process" },
    annotations: [{ content: 'N is Even' }]
  },
  {
    id: "processOddNode",
    offsetX: 440,
    offsetY: 500,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Process" },
    annotations: [{ content: 'N is Odd' }]
  },
  {
    id: "endNode",
    offsetX: 440,
    offsetY: 660,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Terminator" },
    annotations: [{ content: 'End' }]
  }
];

let connectors = [
  {
    id: 'startToInputConnector',
    sourceID: 'startNode',
    targetID: 'inputNode'
  },
  {
    id: 'inputToDecisionConnector',
    sourceID: 'inputNode',
    targetID: 'decisionNode'
  },
  {
    id: 'decisionToProcessEvenConnector',
    sourceID: 'decisionNode',
    targetID: 'processEvenNode',
    annotations: [
      {
        content: 'true',
        style: { fill: 'White'},
      }
    ]
  },
  {
    id: 'decisionToProcessOddConnector',
    sourceID: 'decisionNode',
    targetID: 'processOddNode',
    annotations: [{ content: 'false', 
    style: {fill:'White'},
  }]
  },
  {
    id: 'processOddToEndConnector',
    sourceID: 'processOddNode',
    targetID: 'endNode'
  },
  {
    id: 'processEvenToEndConnector',
    sourceID: 'processEvenNode',
    targetID: 'endNode',
    type: "Orthogonal",
    segments: [
      {
        type: "Orthogonal",
        direction: "Bottom",
        length: 300
      }
    ]
  }
];

import {
  DiagramComponent, NodeConstraints, ConnectorConstraints, DiagramTools,
  UndoRedo
} from '@syncfusion/ej2-vue-diagrams';
import { ButtonComponent } from '@syncfusion/ej2-vue-buttons';

export default {
  name: 'App',
  components: {
    "ejs-diagram": DiagramComponent,
    "ejs-button": ButtonComponent
  },
  data() {
    return {
      width: '1102px',
      height: '602px',
      nodes: nodes,
      connectors: connectors
    };
  },
  methods:{
    onSelect(){
      diagramInstance.select([diagramInstance.nodes[0], diagramInstance.nodes[1],
    diagramInstance.connectors[0]]);
    },
    onClearSelection(){
      diagramInstance.clearSelection();
    }
  },
  mounted: function(){
    diagramInstance = this.$refs.diagramObject.ej2Instances;
    //diagramInstance.tool = DiagramTools.ZoomPan;
    diagramInstance.zoomTo({ type:'ZoomOut', zoomFactor: 0.4 });
  },
  provide: { diagram: [UndoRedo] }
};
</script>

<style>
@import '../node_modules/@syncfusion/ej2-vue-diagrams/styles/material.css';
@import '../node_modules/@syncfusion/ej2-vue-buttons/styles/material.css';

.container{
  text-align: left;
  margin-bottom: 10px;
}

.button{
  margin-right: 10px;
}
</style>