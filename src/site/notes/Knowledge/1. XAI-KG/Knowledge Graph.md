---
{"dg-publish":true,"permalink":"/knowledge/1-xai-kg/knowledge-graph/","title":"Knowledge Graph","pinned":"false"}
---

# References
- [# GraphRAG - Một sự nâng cấp mới của RAG truyền thống chăng?](https://viblo.asia/p/graphrag-mot-su-nang-cap-moi-cua-rag-truyen-thong-chang-EoW4oXRBJml)
- [Graph RAG 1 -Youtube](https://www.youtube.com/watch?v=NJJWAgEUtL8&t=3230s) : Có phần 2 nữa. 
# Questions
- Mình  chưa biết cách nó tổ chức và xây dựng như thế nào
	- Cần survey thêm nha


---
# GraphRAG: Graph-Enhanced Retrieval-Augmented Generation

## Overview

GraphRAG is an advanced question-answering system that combines the power of graph-based knowledge representation with retrieval-augmented generation. It processes input documents to create a rich knowledge graph, which is then used to enhance the retrieval and generation of answers to user queries. The system leverages natural language processing, machine learning, and graph theory to provide more accurate and contextually relevant responses.

## Motivation

Traditional retrieval-augmented generation systems often struggle with maintaining context over long documents and making connections between related pieces of information. GraphRAG addresses these limitations by:

1. Representing knowledge as an interconnected graph, allowing for better preservation of relationships between concepts.
2. Enabling more intelligent traversal of information during the query process.
3. Providing a visual representation of how information is connected and accessed during the answering process.

## Key Components

1. **DocumentProcessor**: Handles the initial processing of input documents, creating text chunks and embeddings.
    
2. **KnowledgeGraph**: Constructs a graph representation of the processed documents, where nodes represent text chunks and edges represent relationships between them.
    
3. **QueryEngine**: Manages the process of answering user queries by leveraging the knowledge graph and vector store.
    
4. **Visualizer**: Creates a visual representation of the graph and the traversal path taken to answer a query.
    

## Method Details

1. **Document Processing**:
    
    - Input documents are split into manageable chunks.
    - Each chunk is embedded using a language model.
    - A vector store is created from these embeddings for efficient similarity search.
2. **Knowledge Graph Construction**:
    
    - Graph nodes are created for each text chunk.
    - Concepts are extracted from each chunk using a combination of NLP techniques and language models.
    - Extracted concepts are lemmatized to improve matching.
    - Edges are added between nodes based on semantic similarity and shared concepts.
    - Edge weights are calculated to represent the strength of relationships.
    - ![](/img/user/assets/images/KG_1.png)
3. **Query Processing**:
    
    - The user query is embedded and used to retrieve relevant documents from the vector store.
    - A priority queue is initialized with the nodes corresponding to the most relevant documents.
    - The system employs a Dijkstra-like algorithm to traverse the knowledge graph:
        - Nodes are explored in order of their priority (strength of connection to the query).
        - For each explored node:
            - Its content is added to the context.
            - The system checks if the current context provides a complete answer.
            - If the answer is incomplete:
                - The node's concepts are processed and added to a set of visited concepts.
                - Neighboring nodes are explored, with their priorities updated based on edge weights.
                - Nodes are added to the priority queue if a stronger connection is found.
    - This process continues until a complete answer is found or the priority queue is exhausted.
    - If no complete answer is found after traversing the graph, the system generates a final answer using the accumulated context and a large language model.
4. **Visualization**:
    
    - The knowledge graph is visualized with nodes representing text chunks and edges representing relationships.
    - Edge colors indicate the strength of relationships (weights).
    - The traversal path taken to answer a query is highlighted with curved, dashed arrows.
    - Start and end nodes of the traversal are distinctly colored for easy identification.

## Benefits of This Approach

1. **Improved Context Awareness**: By representing knowledge as a graph, the system can maintain better context and make connections across different parts of the input documents.
    
2. **Enhanced Retrieval**: The graph structure allows for more intelligent retrieval of information, going beyond simple keyword matching.
    
3. **Explainable Results**: The visualization of the graph and traversal path provides insight into how the system arrived at its answer, improving transparency and trust.
    
4. **Flexible Knowledge Representation**: The graph structure can easily incorporate new information and relationships as they become available.
    
5. **Efficient Information Traversal**: The weighted edges in the graph allow the system to prioritize the most relevant information pathways when answering queries.
    

## Conclusion

GraphRAG represents a significant advancement in retrieval-augmented generation systems. By incorporating a graph-based knowledge representation and intelligent traversal mechanisms, it offers improved context awareness, more accurate retrieval, and enhanced explainability. The system's ability to visualize its decision-making process provides valuable insights into its operation, making it a powerful tool for both end-users and developers. As natural language processing and graph-based AI continue to evolve, systems like GraphRAG pave the way for more sophisticated and capable question-answering technologies.

---

<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.6.0/css/all.min.css" type="text/css"?>
<svg aria-roledescription="flowchart-v2" role="graphics-document document" viewBox="-8 -8 1331.10498046875 1506.921875" style="max-width: 100%;" xmlns="http://www.w3.org/2000/svg" width="100%" id="graph-div" height="100%" xmlns:xlink="http://www.w3.org/1999/xlink"><style>#graph-div{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}#graph-div .error-icon{fill:#552222;}#graph-div .error-text{fill:#552222;stroke:#552222;}#graph-div .edge-thickness-normal{stroke-width:2px;}#graph-div .edge-thickness-thick{stroke-width:3.5px;}#graph-div .edge-pattern-solid{stroke-dasharray:0;}#graph-div .edge-pattern-dashed{stroke-dasharray:3;}#graph-div .edge-pattern-dotted{stroke-dasharray:2;}#graph-div .marker{fill:#333333;stroke:#333333;}#graph-div .marker.cross{stroke:#333333;}#graph-div svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#graph-div .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#333;}#graph-div .cluster-label text{fill:#333;}#graph-div .cluster-label span,#graph-div p{color:#333;}#graph-div .label text,#graph-div span,#graph-div p{fill:#333;color:#333;}#graph-div .node rect,#graph-div .node circle,#graph-div .node ellipse,#graph-div .node polygon,#graph-div .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#graph-div .flowchart-label text{text-anchor:middle;}#graph-div .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#graph-div .node .label{text-align:center;}#graph-div .node.clickable{cursor:pointer;}#graph-div .arrowheadPath{fill:#333333;}#graph-div .edgePath .path{stroke:#333333;stroke-width:2.0px;}#graph-div .flowchart-link{stroke:#333333;fill:none;}#graph-div .edgeLabel{background-color:#e8e8e8;text-align:center;}#graph-div .edgeLabel rect{opacity:0.5;background-color:#e8e8e8;fill:#e8e8e8;}#graph-div .labelBkg{background-color:rgba(232, 232, 232, 0.5);}#graph-div .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#graph-div .cluster text{fill:#333;}#graph-div .cluster span,#graph-div p{color:#333;}#graph-div div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#graph-div .flowchartTitleText{text-anchor:middle;font-size:18px;fill:#333;}#graph-div :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}</style><g><marker orient="auto" markerHeight="12" markerWidth="12" markerUnits="userSpaceOnUse" refY="5" refX="6" viewBox="0 0 10 10" class="marker flowchart" id="graph-div_flowchart-pointEnd"><path style="stroke-width: 1; stroke-dasharray: 1, 0;" class="arrowMarkerPath" d="M 0 0 L 10 5 L 0 10 z"></path></marker><marker orient="auto" markerHeight="12" markerWidth="12" markerUnits="userSpaceOnUse" refY="5" refX="4.5" viewBox="0 0 10 10" class="marker flowchart" id="graph-div_flowchart-pointStart"><path style="stroke-width: 1; stroke-dasharray: 1, 0;" class="arrowMarkerPath" d="M 0 5 L 10 10 L 10 0 z"></path></marker><marker orient="auto" markerHeight="11" markerWidth="11" markerUnits="userSpaceOnUse" refY="5" refX="11" viewBox="0 0 10 10" class="marker flowchart" id="graph-div_flowchart-circleEnd"><circle style="stroke-width: 1; stroke-dasharray: 1, 0;" class="arrowMarkerPath" r="5" cy="5" cx="5"></circle></marker><marker orient="auto" markerHeight="11" markerWidth="11" markerUnits="userSpaceOnUse" refY="5" refX="-1" viewBox="0 0 10 10" class="marker flowchart" id="graph-div_flowchart-circleStart"><circle style="stroke-width: 1; stroke-dasharray: 1, 0;" class="arrowMarkerPath" r="5" cy="5" cx="5"></circle></marker><marker orient="auto" markerHeight="11" markerWidth="11" markerUnits="userSpaceOnUse" refY="5.2" refX="12" viewBox="0 0 11 11" class="marker cross flowchart" id="graph-div_flowchart-crossEnd"><path style="stroke-width: 2; stroke-dasharray: 1, 0;" class="arrowMarkerPath" d="M 1,1 l 9,9 M 10,1 l -9,9"></path></marker><marker orient="auto" markerHeight="11" markerWidth="11" markerUnits="userSpaceOnUse" refY="5.2" refX="-1" viewBox="0 0 11 11" class="marker cross flowchart" id="graph-div_flowchart-crossStart"><path style="stroke-width: 2; stroke-dasharray: 1, 0;" class="arrowMarkerPath" d="M 1,1 l 9,9 M 10,1 l -9,9"></path></marker><g class="root"><g class="clusters"></g><g class="edgePaths"><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-A LE-B" id="L-A-B-0" d="M300.793,39L300.793,43.167C300.793,47.333,300.793,55.667,300.793,63.117C300.793,70.567,300.793,77.133,300.793,80.417L300.793,83.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-B LE-C" id="L-B-C-0" d="M300.793,128L300.793,132.167C300.793,136.333,300.793,144.667,300.793,152.117C300.793,159.567,300.793,166.133,300.793,169.417L300.793,172.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-C LE-D" id="L-C-D-0" d="M227.187,217L211.459,221.167C195.731,225.333,164.276,233.667,148.548,241.117C132.82,248.567,132.82,255.133,132.82,258.417L132.82,261.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-D LE-E" id="L-D-E-0" d="M132.82,306L132.82,310.167C132.82,314.333,132.82,322.667,132.82,330.117C132.82,337.567,132.82,344.133,132.82,347.417L132.82,350.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-E LE-F" id="L-E-F-0" d="M132.82,395L132.82,401.167C132.82,407.333,132.82,419.667,132.82,443.699C132.82,467.731,132.82,503.461,132.82,521.327L132.82,539.192"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-C LE-G" id="L-C-G-0" d="M374.399,217L390.127,221.167C405.855,225.333,437.31,233.667,453.038,241.117C468.766,248.567,468.766,255.133,468.766,258.417L468.766,261.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-G LE-H" id="L-G-H-0" d="M468.766,306L468.766,310.167C468.766,314.333,468.766,322.667,468.766,330.117C468.766,337.567,468.766,344.133,468.766,347.417L468.766,350.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-H LE-I" id="L-H-I-0" d="M468.766,395L468.766,401.167C468.766,407.333,468.766,419.667,468.766,443.699C468.766,467.731,468.766,503.461,468.766,521.327L468.766,539.192"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-I LE-J" id="L-I-J-0" d="M468.766,583.492L468.766,602.241C468.766,620.99,468.766,658.487,468.766,682.519C468.766,706.551,468.766,717.118,468.766,722.401L468.766,727.684"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-J LE-K" id="L-J-K-0" d="M468.766,771.984L468.766,776.151C468.766,780.318,468.766,788.651,468.766,796.101C468.766,803.551,468.766,810.118,468.766,813.401L468.766,816.684"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-K LE-L" id="L-K-L-0" d="M468.766,860.984L468.766,865.151C468.766,869.318,468.766,877.651,468.766,898.512C468.766,919.374,468.766,952.764,468.766,969.458L468.766,986.153"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-M LE-N" id="L-M-N-0" d="M1049.996,39L1049.996,43.167C1049.996,47.333,1049.996,55.667,1049.996,63.117C1049.996,70.567,1049.996,77.133,1049.996,80.417L1049.996,83.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-N LE-O" id="L-N-O-0" d="M1049.996,128L1049.996,132.167C1049.996,136.333,1049.996,144.667,1049.996,152.117C1049.996,159.567,1049.996,166.133,1049.996,169.417L1049.996,172.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-O LE-P" id="L-O-P-0" d="M1049.996,217L1049.996,221.167C1049.996,225.333,1049.996,233.667,1049.996,241.117C1049.996,248.567,1049.996,255.133,1049.996,258.417L1049.996,261.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-P LE-Q" id="L-P-Q-0" d="M1049.996,306L1049.996,310.167C1049.996,314.333,1049.996,322.667,1049.996,330.117C1049.996,337.567,1049.996,344.133,1049.996,347.417L1049.996,350.7"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-Q LE-R" id="L-Q-R-0" d="M951.254,390.092L903.99,397.077C856.727,404.061,762.199,418.031,735.016,439.851C707.832,461.671,747.992,491.342,768.072,506.178L788.152,521.014"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-R LE-Q" id="L-R-Q-0" d="M900.837,523.259L920.476,508.049C940.114,492.839,979.391,462.42,1002.02,441.816C1024.65,421.212,1030.632,410.423,1033.623,405.029L1036.614,399.635"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-R LE-S" id="L-R-S-0" d="M847.078,659.484L846.995,665.568C846.911,671.651,846.745,683.818,846.661,695.184C846.578,706.551,846.578,717.118,846.578,722.401L846.578,727.684"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-S LE-T" id="L-S-T-0" d="M846.578,771.984L846.578,776.151C846.578,780.318,846.578,788.651,846.578,796.101C846.578,803.551,846.578,810.118,846.578,813.401L846.578,816.684"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-T LE-U" id="L-T-U-0" d="M846.578,860.984L846.578,865.151C846.578,869.318,846.578,877.651,846.644,885.185C846.71,892.718,846.842,899.452,846.908,902.819L846.974,906.185"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-U LE-W" id="L-U-W-0" d="M894.988,1063.512L907.928,1077.581C920.868,1091.649,946.748,1119.785,959.689,1139.137C972.629,1158.489,972.629,1169.055,972.629,1174.339L972.629,1179.622"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-U LE-V" id="L-U-V-0" d="M801.682,1066.026L790.175,1079.675C778.668,1093.324,755.654,1120.623,744.148,1139.556C732.641,1158.489,732.641,1169.055,732.641,1174.339L732.641,1179.622"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-V LE-X" id="L-V-X-0" d="M732.641,1223.922L732.641,1228.089C732.641,1232.255,732.641,1240.589,732.641,1248.039C732.641,1255.489,732.641,1262.055,732.641,1265.339L732.641,1268.622"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-X LE-Y" id="L-X-Y-0" d="M732.641,1312.922L732.641,1317.089C732.641,1321.255,732.641,1329.589,732.641,1337.039C732.641,1344.489,732.641,1351.055,732.641,1354.339L732.641,1357.622"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-Y LE-Z" id="L-Y-Z-0" d="M732.641,1401.922L732.641,1406.089C732.641,1410.255,732.641,1418.589,762.905,1426.89C793.169,1435.191,853.697,1443.46,883.961,1447.594L914.225,1451.729"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-Z LE-Q" id="L-Z-Q-0" d="M1170.875,1451.922L1194.913,1447.755C1218.952,1443.589,1267.029,1435.255,1291.067,1423.672C1315.105,1412.089,1315.105,1397.255,1315.105,1382.422C1315.105,1367.589,1315.105,1352.755,1315.105,1337.922C1315.105,1323.089,1315.105,1308.255,1315.105,1293.422C1315.105,1278.589,1315.105,1263.755,1315.105,1248.922C1315.105,1234.089,1315.105,1219.255,1315.105,1202.422C1315.105,1185.589,1315.105,1166.755,1315.105,1134.51C1315.105,1102.266,1315.105,1056.609,1315.105,1012.953C1315.105,969.297,1315.105,927.641,1315.105,899.396C1315.105,871.151,1315.105,856.318,1315.105,841.484C1315.105,826.651,1315.105,811.818,1315.105,796.984C1315.105,782.151,1315.105,767.318,1315.105,750.484C1315.105,733.651,1315.105,714.818,1315.105,683.402C1315.105,651.987,1315.105,607.99,1315.105,563.992C1315.105,519.995,1315.105,475.997,1287.034,448.016C1258.963,420.035,1202.82,408.07,1174.749,402.087L1146.678,396.105"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-Q LE-AA" id="L-Q-AA-0" d="M1049.996,395L1049.996,401.167C1049.996,407.333,1049.996,419.667,1064.026,440.625C1078.056,461.583,1106.115,491.167,1120.145,505.959L1134.175,520.75"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-AA LE-AB" id="L-AA-AB-0" d="M1175.652,642.219L1175.569,651.18C1175.486,660.141,1175.319,678.063,1175.236,692.307C1175.152,706.551,1175.152,717.118,1175.152,722.401L1175.152,727.684"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-AA LE-Q" id="L-AA-Q-0" d="M1198.301,509.414L1203.489,496.512C1208.677,483.61,1219.054,457.805,1205.5,439.001C1191.946,420.197,1154.463,408.395,1135.721,402.493L1116.98,396.592"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-W LE-AC" id="L-W-AC-0" d="M972.629,1223.922L972.629,1228.089C972.629,1232.255,972.629,1240.589,984.972,1248.656C997.315,1256.723,1022.001,1264.524,1034.345,1268.424L1046.688,1272.325"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-AB LE-AC" id="L-AB-AC-0" d="M1175.152,771.984L1175.152,776.151C1175.152,780.318,1175.152,788.651,1175.152,800.234C1175.152,811.818,1175.152,826.651,1175.152,841.484C1175.152,856.318,1175.152,871.151,1175.152,899.396C1175.152,927.641,1175.152,969.297,1175.152,1012.953C1175.152,1056.609,1175.152,1102.266,1175.152,1134.51C1175.152,1166.755,1175.152,1185.589,1175.152,1202.422C1175.152,1219.255,1175.152,1234.089,1170.091,1245.155C1165.03,1256.222,1154.908,1263.522,1149.847,1267.172L1144.786,1270.822"></path><path marker-end="url(#graph-div_flowchart-pointEnd)" style="fill:none;" class="edge-thickness-normal edge-pattern-solid flowchart-link LS-AC LE-AD" id="L-AC-AD-0" d="M1113.449,1312.922L1113.449,1317.089C1113.449,1321.255,1113.449,1329.589,1113.449,1337.039C1113.449,1344.489,1113.449,1351.055,1113.449,1354.339L1113.449,1357.622"></path></g><g class="edgeLabels"><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g transform="translate(1018.66796875, 432)" class="edgeLabel"><g transform="translate(-11.328125, -12)" class="label"><foreignObject height="24" width="22.65625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">Yes</span></div></foreignObject></g></g><g transform="translate(846.578125, 695.984375)" class="edgeLabel"><g transform="translate(-9.3984375, -12)" class="label"><foreignObject height="24" width="18.796875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">No</span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g transform="translate(972.62890625, 1147.921875)" class="edgeLabel"><g transform="translate(-11.328125, -12)" class="label"><foreignObject height="24" width="22.65625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">Yes</span></div></foreignObject></g></g><g transform="translate(732.640625, 1147.921875)" class="edgeLabel"><g transform="translate(-9.3984375, -12)" class="label"><foreignObject height="24" width="18.796875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">No</span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g transform="translate(1175.15234375, 695.984375)" class="edgeLabel"><g transform="translate(-11.328125, -12)" class="label"><foreignObject height="24" width="22.65625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">Yes</span></div></foreignObject></g></g><g transform="translate(1229.4296875, 432)" class="edgeLabel"><g transform="translate(-9.3984375, -12)" class="label"><foreignObject height="24" width="18.796875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel">No</span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g transform="translate(0, 0)" class="label"><foreignObject height="0" width="0"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g transform="translate(300.79296875, 19.5)" data-id="A" data-node="true" id="flowchart-A-320" class="node default default flowchart-label"><rect height="39" width="50.015625" y="-19.5" x="-25.0078125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-17.5078125, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="35.015625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Start</span></div></foreignObject></g></g><g transform="translate(300.79296875, 108.5)" data-id="B" data-node="true" id="flowchart-B-321" class="node default default flowchart-label"><rect height="39" width="135.65625" y="-19.5" x="-67.828125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-60.328125, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="120.65625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Input Documents</span></div></foreignObject></g></g><g transform="translate(300.79296875, 197.5)" data-id="C" data-node="true" id="flowchart-C-323" class="node default default flowchart-label"><rect height="39" width="154.546875" y="-19.5" x="-77.2734375" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-69.7734375, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="139.546875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">DocumentProcessor</span></div></foreignObject></g></g><g transform="translate(132.8203125, 286.5)" data-id="D" data-node="true" id="flowchart-D-325" class="node default default flowchart-label"><rect height="39" width="170.375" y="-19.5" x="-85.1875" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-77.6875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="155.375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Split Text into Chunks</span></div></foreignObject></g></g><g transform="translate(132.8203125, 375.5)" data-id="E" data-node="true" id="flowchart-E-327" class="node default default flowchart-label"><rect height="39" width="265.640625" y="-19.5" x="-132.8203125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-125.3203125, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="250.640625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Create Embeddings for Each Chunk</span></div></foreignObject></g></g><g transform="translate(132.8203125, 563.9921875)" data-id="F" data-node="true" id="flowchart-F-329" class="node default default flowchart-label"><rect height="39" width="144.375" y="-19.5" x="-72.1875" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-64.6875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="129.375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Build Vector Store</span></div></foreignObject></g></g><g transform="translate(468.765625, 286.5)" data-id="G" data-node="true" id="flowchart-G-331" class="node default default flowchart-label"><rect height="39" width="135.171875" y="-19.5" x="-67.5859375" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-60.0859375, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="120.171875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">KnowledgeGraph</span></div></foreignObject></g></g><g transform="translate(468.765625, 375.5)" data-id="H" data-node="true" id="flowchart-H-333" class="node default default flowchart-label"><rect height="39" width="306.25" y="-19.5" x="-153.125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-145.625, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="291.25"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Create Graph Nodes for Each Text Chunk</span></div></foreignObject></g></g><g transform="translate(468.765625, 563.9921875)" data-id="I" data-node="true" id="flowchart-I-335" class="node default default flowchart-label"><rect height="39" width="273.609375" y="-19.5" x="-136.8046875" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-129.3046875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="258.609375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Extract Concepts using LLM and NLP</span></div></foreignObject></g></g><g transform="translate(468.765625, 752.484375)" data-id="J" data-node="true" id="flowchart-J-337" class="node default default flowchart-label"><rect height="39" width="238.40625" y="-19.5" x="-119.203125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-111.703125, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="223.40625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Lemmatize Extracted Concepts</span></div></foreignObject></g></g><g transform="translate(468.765625, 841.484375)" data-id="K" data-node="true" id="flowchart-K-339" class="node default default flowchart-label"><rect height="39" width="278.890625" y="-19.5" x="-139.4453125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-131.9453125, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="263.890625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Compute Similarities Between Nodes</span></div></foreignObject></g></g><g transform="translate(468.765625, 1010.953125)" data-id="L" data-node="true" id="flowchart-L-341" class="node default default flowchart-label"><rect height="39" width="455.6875" y="-19.5" x="-227.84375" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-220.34375, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="440.6875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Add Weighted Edges Based on Similarity and Shared Concepts</span></div></foreignObject></g></g><g transform="translate(1049.99609375, 19.5)" data-id="M" data-node="true" id="flowchart-M-342" class="node default default flowchart-label"><rect height="39" width="94.015625" y="-19.5" x="-47.0078125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-39.5078125, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="79.015625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">User Query</span></div></foreignObject></g></g><g transform="translate(1049.99609375, 108.5)" data-id="N" data-node="true" id="flowchart-N-343" class="node default default flowchart-label"><rect height="39" width="104.765625" y="-19.5" x="-52.3828125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-44.8828125, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="89.765625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">QueryEngine</span></div></foreignObject></g></g><g transform="translate(1049.99609375, 197.5)" data-id="O" data-node="true" id="flowchart-O-345" class="node default default flowchart-label"><rect height="39" width="357.4375" y="-19.5" x="-178.71875" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-171.21875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="342.4375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Retrieve Relevant Documents from Vector Store</span></div></foreignObject></g></g><g transform="translate(1049.99609375, 286.5)" data-id="P" data-node="true" id="flowchart-P-347" class="node default default flowchart-label"><rect height="39" width="336.71875" y="-19.5" x="-168.359375" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-160.859375, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="321.71875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Initialize Priority Queue with Relevant Nodes</span></div></foreignObject></g></g><g transform="translate(1049.99609375, 375.5)" data-id="Q" data-node="true" id="flowchart-Q-349" class="node default default flowchart-label"><rect height="39" width="197.484375" y="-19.5" x="-98.7421875" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-91.2421875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="182.484375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Pop Highest Priority Node</span></div></foreignObject></g></g><g transform="translate(846.578125, 563.9921875)" data-id="R" data-node="true" id="flowchart-R-351" class="node default default flowchart-label"><polygon style="" transform="translate(-94.9921875,94.9921875)" class="label-container" points="94.9921875,0 189.984375,-94.9921875 94.9921875,-189.984375 0,-94.9921875"></polygon><g transform="translate(-67.9921875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="135.984375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Better Path Found?</span></div></foreignObject></g></g><g transform="translate(846.578125, 752.484375)" data-id="S" data-node="true" id="flowchart-S-355" class="node default default flowchart-label"><rect height="39" width="257.375" y="-19.5" x="-128.6875" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-121.1875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="242.375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Process Node and Update Context</span></div></foreignObject></g></g><g transform="translate(846.578125, 841.484375)" data-id="T" data-node="true" id="flowchart-T-357" class="node default default flowchart-label"><rect height="39" width="217.59375" y="-19.5" x="-108.796875" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-101.296875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="202.59375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Check Answer Completeness</span></div></foreignObject></g></g><g transform="translate(846.578125, 1010.953125)" data-id="U" data-node="true" id="flowchart-U-359" class="node default default flowchart-label"><polygon style="" transform="translate(-99.96875,99.96875)" class="label-container" points="99.96875,0 199.9375,-99.96875 99.96875,-199.9375 0,-99.96875"></polygon><g transform="translate(-72.96875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="145.9375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Is Answer Complete?</span></div></foreignObject></g></g><g transform="translate(972.62890625, 1204.421875)" data-id="W" data-node="true" id="flowchart-W-361" class="node default default flowchart-label"><rect height="39" width="176.8125" y="-19.5" x="-88.40625" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-80.90625, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="161.8125"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Generate Final Answer</span></div></foreignObject></g></g><g transform="translate(732.640625, 1204.421875)" data-id="V" data-node="true" id="flowchart-V-363" class="node default default flowchart-label"><rect height="39" width="178.9375" y="-19.5" x="-89.46875" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-81.96875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="163.9375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Process Node Concepts</span></div></foreignObject></g></g><g transform="translate(732.640625, 1293.421875)" data-id="X" data-node="true" id="flowchart-X-365" class="node default default flowchart-label"><rect height="39" width="190.296875" y="-19.5" x="-95.1484375" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-87.6484375, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="175.296875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Update Visited Concepts</span></div></foreignObject></g></g><g transform="translate(732.640625, 1382.421875)" data-id="Y" data-node="true" id="flowchart-Y-367" class="node default default flowchart-label"><rect height="39" width="144.046875" y="-19.5" x="-72.0234375" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-64.5234375, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="129.046875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Explore Neighbors</span></div></foreignObject></g></g><g transform="translate(1058.375, 1471.421875)" data-id="Z" data-node="true" id="flowchart-Z-369" class="node default default flowchart-label"><rect height="39" width="277.796875" y="-19.5" x="-138.8984375" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-131.3984375, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="262.796875"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Update Distances and Priority Queue</span></div></foreignObject></g></g><g transform="translate(1175.15234375, 563.9921875)" data-id="AA" data-node="true" id="flowchart-AA-373" class="node default default flowchart-label"><polygon style="" transform="translate(-77.7265625,77.7265625)" class="label-container" points="77.7265625,0 155.453125,-77.7265625 77.7265625,-155.453125 0,-77.7265625"></polygon><g transform="translate(-50.7265625, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="101.453125"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Queue Empty?</span></div></foreignObject></g></g><g transform="translate(1175.15234375, 752.484375)" data-id="AB" data-node="true" id="flowchart-AB-375" class="node default default flowchart-label"><rect height="39" width="205.90625" y="-19.5" x="-102.953125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-95.453125, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="190.90625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Generate Answer with LLM</span></div></foreignObject></g></g><g transform="translate(1113.44921875, 1293.421875)" data-id="AC" data-node="true" id="flowchart-AC-379" class="node default default flowchart-label"><rect height="39" width="213.90625" y="-19.5" x="-106.953125" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-99.453125, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="198.90625"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">Return Final Answer to User</span></div></foreignObject></g></g><g transform="translate(1113.44921875, 1382.421875)" data-id="AD" data-node="true" id="flowchart-AD-383" class="node default default flowchart-label"><rect height="39" width="41.234375" y="-19.5" x="-20.6171875" ry="0" rx="0" style="" class="basic label-container"></rect><g transform="translate(-13.1171875, -12)" style="" class="label"><rect></rect><foreignObject height="24" width="26.234375"><div style="display: inline-block; white-space: nowrap;" xmlns="http://www.w3.org/1999/xhtml"><span class="nodeLabel">End</span></div></foreignObject></g></g></g></g></g></svg>

---
### Luồng hoạt động chính

Hệ thống được chia thành các thành phần sau:

1. **DocumentProcessor**:
    
    - Xử lý tài liệu đầu vào:
        - Chia nhỏ tài liệu thành các đoạn (chunks).
        - Tạo embeddings cho các đoạn này bằng mô hình nhúng (embedding model).
        - Tạo một kho lưu trữ vector để tìm kiếm tương tự.
2. **KnowledgeGraph**:
    
    - Xây dựng đồ thị tri thức từ các đoạn văn bản:
        - Các đoạn văn bản là các nút của đồ thị.
        - Các mối quan hệ giữa các nút được biểu diễn bằng các cạnh, dựa trên độ tương đồng ngữ nghĩa và khái niệm chung.
        - Các cạnh được gán trọng số dựa trên mức độ quan hệ.
3. **QueryEngine**:
    
    - Xử lý câu hỏi người dùng:
        - Tìm kiếm các tài liệu liên quan nhất từ kho vector.
        - Sử dụng thuật toán tương tự Dijkstra để duyệt qua đồ thị tri thức.
        - Kết hợp thông tin từ các nút được duyệt để tạo câu trả lời.
        - Nếu không tìm được câu trả lời đầy đủ, LLM sẽ được sử dụng để tạo câu trả lời cuối cùng.
4. **Visualizer**:
    
    - Trực quan hóa đồ thị và đường đi:
        - Đồ thị được hiển thị với các nút và cạnh.
        - Đường đi qua các nút để trả lời câu hỏi được đánh dấu.
5. **GraphRAG Class**:
    
    - Là lớp chính quản lý toàn bộ luồng hoạt động:
        - Tích hợp các thành phần trên.
        - Xử lý tài liệu và câu hỏi một cách liền mạch.

### Các bước chạy cơ bản

1. **Tải dữ liệu**:
    
    - Tài liệu như file PDF được tải vào hệ thống.
    - Các đoạn văn bản được xử lý thành các chunk.
2. **Xây dựng đồ thị tri thức**:
    
    - Các mối quan hệ giữa nội dung được trích xuất và biểu diễn dưới dạng đồ thị.
3. **Truy vấn thông tin**:
    
    - Khi người dùng đặt câu hỏi, hệ thống sẽ tìm kiếm thông tin trong đồ thị tri thức.
    - Sử dụng các thuật toán để duyệt qua đồ thị và tạo câu trả lời.
4. **Hiển thị kết quả**:
    
    - Đồ thị tri thức và các đường đi có thể được trực quan hóa.
---

# Thực hiện

## Prompt
![](/img/user/assets/images/KG1.png)
![](/img/user/assets/images/KG2.png)
```python
def CreatePrompt(text: str) -> str:

  

    prompt = f"""

        # Nhiệm vụ: Trích xuất từ khóa nội dung (topics) và các thẻ (tags) từ đoạn văn về trường đại học.

  

        ## Mục tiêu:

        Cho một đoạn văn bất kỳ, hãy trích xuất:

        1. topics: danh sách từ khóa có trong nội dung văn bản(lấy từ văn bản). các từ khóa này có thể là tên trường, tên ngành, chuyên ngành,...

        2. tags: các nhãn cụ thể được chọn từ danh sách tag chuẩn bên dưới.

        ## Định dạng đầu ra:

  

        topics: [từ khóa 1, từ khóa 2, ...]  

        tags: [tag 1, tag 2, ...]

        Ví dụ:

        topics: [Đại học ABC, Trí tuệ nhân tạo, An ninh mạng, thời gian đạo tạo, điều kiện xét tuyển]

        tags: [Ngành đào tạo, Tên ngành, Chuyên ngành, Thời gian đào tạo, Điều kiện tuyển sinh, Xét điểm thi, Xét học bạ, Đại học ABC]

  

        Chú ý:

        - Không có hiện json ở đầu ra.

        - số lượng từ khóa trong topics không quá 12 từ khóa.

        - số lượng tag trong tags không quá 12 tags.

        - không có dấu câu ở cuối danh sách từ khóa và danh sách tag.

        - không có từ khóa nào lặp lại trong danh sách từ khóa và danh sách tag.

        -------

        ## Danh sách tags chuẩn:

  

        - Thông tin trường: Tên trường, Địa chỉ, Sứ mệnh, Mục tiêu, Website, Thông tin liên hệ, Số điện thoại, Email  

        - Ngành đào tạo, Tên ngành, Chuyên ngành, Mô tả ngành học, Thời gian đào tạo, Điều kiện tuyển sinh  

        - Chỉ tiêu tuyển sinh, Ngành học, Số lượng chỉ tiêu, Hình thức xét tuyển, Xét điểm thi, Xét học bạ, Thi đánh giá năng lực, Chứng chỉ quốc tế  

        - Điều kiện nhập học, Yêu cầu đầu vào, Học phí, Chính sách học bổng, Hỗ trợ tài chính  

        - Lịch tuyển sinh, Thời gian nhận hồ sơ, Thời gian thi, Thời gian công bố kết quả, Thời gian nhập học, Lịch thi riêng  

        - Chính sách hỗ trợ học tập, Chính sách việc làm, Hỗ trợ sinh viên, Vay vốn học tập, Hợp tác doanh nghiệp, học bổng, thực tập, việc làm

        - Cơ hội nghề nghiệp, Môi trường làm việc, Đối tác doanh nghiệp, Thực tập sinh, Việc làm sau tốt nghiệp  

        - Cơ sở vật chất, Thư viện, Phòng thí nghiệm, Ký túc xá, Sân thể thao, Cơ sở chính, Cơ sở vệ tinh, Câu lạc bộ, Hoạt động ngoại khóa  

        - Chương trình quốc tế, Du học, Liên kết đào tạo quốc tế, Thạc sĩ, Tiến sĩ  

        - Ngày hội tuyển sinh, Hội thảo hướng nghiệp, Webinar, Livestream tuyển sinh

  

        ---

  

        ## Ví dụ:

  

        ### Input 1:

        Đại học ABC có các ngành đào tạo đa dạng, trong đó nổi bật là ngành Công nghệ thông tin với nhiều chuyên ngành như Trí tuệ nhân tạo và An ninh mạng. Thời gian đào tạo 4 năm. Điều kiện xét tuyển gồm điểm thi tốt nghiệp THPT và học bạ.

  

        ### Kết quả:

        topics: [Đại học ABC, Trí tuệ nhân tạo, An ninh mạng, thời gian đạo tạo, điều kiện xét tuyển]  

        tags: [Ngành đào tạo, Tên ngành, Chuyên ngành, Thời gian đào tạo, Điều kiện tuyển sinh, Xét điểm thi, Xét học bạ, Đại học ABC]

  

        ---

  

        ### Input 2:

        Trường Đại học XYZ tọa lạc tại Quận 3, TP.HCM. Trường có sứ mệnh đào tạo nguồn nhân lực chất lượng cao cho khu vực miền Nam. Mọi thông tin tuyển sinh được công bố tại website chính thức.

  

        ### Kêt quả:

        topics: [Đại học XYZ, thông tin trường, địa chỉ, sứ mệnh, website tuyển sinh]  

        tags: [Tên trường, Địa chỉ, Sứ mệnh, Website, Thông tin tuyển sinh]

        ---

        Đây là nội dung văn bản cần phân tích:

        {text}

        """

    return prompt
```


![](/img/user/assets/images/KG3.png)

---
# Kiến thức thêm về Knowledge graph
