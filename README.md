# testPage
*My first html and css page: [Dog Page]{https://moranarm.github.io/testPage/dogPage/index.html}
Some rather difficult code implementing the NEAT algorithm in my Pacman game
'''Java
void addNodeMutation(ArrayList<Counter> innovation){//connection gene is randomly chosen and replaced with two new connections
    if(connections.size() == 0){
      addConnectionMutation(innovation);
      return;
    }
    int randomConnection = floor(random(connections.size()));
    while(connections.get(randomConnection).inNode == nodes.get(biasNode) && connections.size()!=1){//keep the bias connected
      randomConnection = floor(random(connections.size()));
    }
    connections.get(randomConnection).disable();//disable it
    int newNodeNum = nextNode;
    nodes.add(new NodeGene(newNodeNum));
    nextNode++;
    int connectionInnovationNum = getInnovationNumber(innovation, connections.get(randomConnection).inNode, getNode(newNodeNum));//add a new connection with weight of 1
    connections.add(new ConnectionGene(connections.get(randomConnection).inNode, getNode(newNodeNum), 1, connectionInnovationNum));
    connectionInnovationNum = getInnovationNumber(innovation, getNode(newNodeNum), connections.get(randomConnection).outNode);
    //adds a new connection from the new node that has the same weight as the disabled connection
    connections.add(new ConnectionGene(getNode(newNodeNum), connections.get(randomConnection).outNode, connections.get(randomConnection).weight, connectionInnovationNum));
    getNode(newNodeNum).layer = connections.get(randomConnection).inNode.layer+1;
    connectionInnovationNum = getInnovationNumber(innovation, nodes.get(biasNode), getNode(newNodeNum));
    connections.add(new ConnectionGene(nodes.get(biasNode), getNode(newNodeNum), 0, connectionInnovationNum));//connect the new node to the bias with a weight of 0
    //increment the layer numbers of all layers greater than or equal to this node
    if(getNode(newNodeNum).layer == connections.get(randomConnection).outNode.layer){
      for(int i=0; i<nodes.size()-1; i++){//do not include the newest node
        if(nodes.get(i).layer >= getNode(newNodeNum).layer){
          nodes.get(i).layer++;
        }
      }
      layers++;
    }
    connectNodes();
  }
