"""
A solution to a ROSALIND bioinformatics problem.
Problem Title: Reconstruct a String from its k-mer Composition
Rosalind ID: BA3H
URL: http://rosalind.info/problems/ba3h/
"""

def graph2(adj):
    #Add all nodes as keys (even if they have not output degree > 0)
    adjDict=dict()
    for x in adj:
        x=x.split()
        adjDict[x[0]]=x[2].split(",")
        for node in x[2].split(","):
            if node not in adjDict.keys():
                adjDict[node]=[]
    return adjDict

def cycles(adjDict):
    edgesDict=dict()
    for key in adjDict.keys():
        edgesDict[key]=[(key,value) for value in adjDict[key]]
    cycles=[]
    import random
    index=random.randint(0,len(adjDict.keys())-1)
    v0=[key for key in adjDict.keys()][index]
    def oneCycle():
        cyclev0 = []
        for node in adjDict[v0]:
            if (v0, node) not in cyclev0:
                if (v0, node) in edgesDict[v0]:
                    cyclev0.append((v0, node))
                    edgesDict[v0].remove((v0, node))
                    nodesv0.append(node)
                    startNode = node
                    break
        while (startNode != v0):
            for node in adjDict[startNode]:
                if (startNode, node) not in cyclev0:
                    if (startNode, node) in edgesDict[startNode]:
                        cyclev0.append((startNode, node))
                        edgesDict[startNode].remove((startNode, node))
                        nodesv0.append(node)
                        startNode = node
                        break
        cycles.append(cyclev0)

    nodesv0 = [v0]
    oneCycle()
    flag=0
    for i in range(1, len(nodesv0)):
        if len(edgesDict[nodesv0[i]]) > 0:
            v0 = nodesv0[i]
            flag = 1
            break
    while (flag > 0):
        nodesv0 = [v0]
        oneCycle()
        cycles = [EulerianCycle(cycles)]
        flag=0
        nodesv0=[cycles[0][0][0]]
        for element in cycles[0]:
            nodesv0.append(element[1])
        for i in range(1, len(nodesv0)):
            if len(edgesDict[nodesv0[i]]) > 0:
                v0 = nodesv0[i]
                flag = 1
                break
    return  cycles

def EulerianCycle(cycles):
    EulerianCycle=[]
    remove = []
    for element in cycles[0]:
        EulerianCycle.append(element)
        remove.append(element)
        if element[1] == cycles[1][0][0]:
            break
    for element in remove:
        cycles[0].remove(element)

    for i in range(1,-1,-1):
        remove=[]
        for element in cycles[i]:
            EulerianCycle.append(element)
            remove.append(element)
        for element in remove:
            cycles[i].remove(element)

    return EulerianCycle


def rosalindOutputToGraph(ecycle):
    D = {}
    nodes = ecycle.split("->")
    for i in range(0, len(nodes) - 1):
        first = nodes[i]
        second = nodes[i + 1]
        if first in D:
            D[first].append(second)
        else:
            D[first] = [second]
    for first in D:
        D[first] = sorted(D[first])
    return D

def unbalancedDegrees(graph):
    unbalancedNodesDict=dict()
    for key in graph.keys():
        inputDeg=0
        for key2 in graph.keys():
            if key2!=key:
                for node in graph[key2]:
                    if node==key:
                        inputDeg=inputDeg+1
        if len(graph[key])!=inputDeg:
            unbalancedNodesDict[key]=inputDeg-len(graph[key])
    unbalancedNodes=[0,0]
    for key in unbalancedNodesDict.keys():
        if unbalancedNodesDict[key]>0:
            unbalancedNodes[0]=key
        if unbalancedNodesDict[key]<0:
            unbalancedNodes[1]=key
    return unbalancedNodes

def balancedGraph(graph):
    startEnd=unbalancedDegrees(graph)
    graph[startEnd[0]].append(startEnd[1])
    return [graph,startEnd]

def EulerianPath2(inlines):
    balanced=balancedGraph(graph2(inlines))
    cycle=cycles(balanced[0])
    path1=[]
    path2=[]
    path=path1
    for x in cycle[0]:
        if x==(balanced[1][0],balanced[1][1]):
            l=len(balanced[1][0])
            path=path2
        path.append(x)
    return printEulerOverlap([path2+path1])[l:]

def printEulerOverlap(eulerianCycle):
    nodes=[eulerianCycle[0][0][0]]
    nodes.append(prefix(eulerianCycle[0][0][1]))
    for x in eulerianCycle[0]:
        nodes.append(x[1][-1])
    return ("".join(nodes))


def prefix(pattern):
    """substring of pattern without  the last letter"""
    return pattern[0:len(pattern)-1]

def suffix(pattern):
    #substring of pattern without first letter
    return pattern[1:]

def graphFromStrings(inlines):
    text=""
    nodes=[]
    adj=dict()
    for x in inlines:
        nodes.append(prefix(x))
        nodes.append(suffix(x))
    nodes=list(set(nodes))
    for node in nodes:
        adj[node]=[]
    for node in nodes:
        for x in inlines:
            if prefix(x)==node:
                adj[node].append(suffix(x))
    for key in adj.keys():
        if len(adj[key])>0:
            text=text+key+" -> "+",".join(adj[key])+"\n"
    return text[:-1]


if __name__ == '__main__':

    x = '''4
CTTA
ACCA
TACC
GGCT
GCTT
TTAC'''
    inlines=x.split("\n")[1:]
    graph=graphFromStrings(inlines).split("\n")
    res=EulerianPath2(graph)
    print(res)
    print(res=="GGCTTACCA")
