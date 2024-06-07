The project aims to create a distributed system that converts a series of images into a video. The rendering
process will be distributed across multiple nodes using the Ambassador and Circuit Breaker patterns to ensure
reliability and fault tolerance.
Architecture Overview
Ambassador: The Ambassador acts as an intermediary between the sets of images and the rendering nodes.
It handles job distribution, node selection, and implements the Circuit Breaker pattern to manage node
failures.
1. The Ambassador will separate the list of images to render. Use the CAM_FRONT
zip in the drive’s classroom.
2. The Ambassador IS the server and will handle new connections.
3. For the Ambassador, design an identification system for the sets of images
each node will render.
Like the past project, define a set of status flags that tells you how the
rendering process is going for each set. E.g.
A = Available
B = Processing
C = Fail
D = Counted
“0”: {‘Status’: A,
‘Images’: [‘path1’, ‘path2’]}
“1”: {‘Status’: B,
‘Images’: [‘path3’, ‘path4’]}

4. The Ambassador will handle load balancing, and routing to the nodes.
 For load balancing use a dictionary to separate the list of images with
their respective identifier as shown before.
 For accepting new nodes use a dictionary to keep track of the available
nodes and the ones that already ended their task or exit with failure
(the status will be obtained by the outputs of circuit breaker).

5. When a new node is accepted by the Ambassador, a new thread and its respective
circuit breaker instance will be opened.
6. The Ambassador will be able to manage the outputs from the circuit breaker to
take decisions. Remember that no set of images must be left uncounted.
7. After all the loads have been rendered (multiple “mini videos” will be
created), the Ambassador will merge these videos into one last main video.
Distributed Systems: 3rd Module

Circuit Breaker (CB)
1. All new connections with the Ambassador will be handled by circuit breaker.
Failure and success handling will be managed here.
2. CB will receive at least two inputs, the identifier that the ambassador
assigned to the node, and the information of the connection to the node.
3. The errors that the CB will handle are up to you to decide, however, it’s
expected to handle at least disconnections and errors with the node.
Remember to set either a timeout or a series of “try’s” when handling the
errors from the node.
4. The outputs from CB must be handled by the Ambassador. Some failures must be
communicated in some way to the Ambassador. At least your CB architecture must
send the following outputs:
a. If there is a disconnection from the node, CB will send a status of
failure.
b. If the counting was successful, CB will send to the Ambassador both the
status and the path to the rendered “mini” video.
