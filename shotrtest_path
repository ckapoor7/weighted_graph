import java.io.*;  
import java.lang.*;  
import java.util.*;  
  
/* 
 * This class represents a vertex in an undirected weighted graph. 
 */  
class vertex implements Comparable <vertex> {  
        private int vertex_id;  
        private int predecessor_vertex_id;  
        private double distance;  
        static final private int VERTEX_IS_SOURCE = -1;  
  
        public vertex (int vertex_id, int predecessor_vertex_id, double distance) {  
                this.vertex_id             = vertex_id;  
                this.predecessor_vertex_id = predecessor_vertex_id;  
                this.distance              = distance;  
        }  
  
        /* 
         * this function is called to create a disconnected vertex. 
         */  
        public static vertex create_disconnected_vertex(int vertex_id) {  
                vertex v = new vertex(vertex_id, Integer.MAX_VALUE, Double.POSITIVE_INFINITY);  
                return v;  
        }  
  
        /* 
         * this function is called to create a source vertex in the 
         * graph. 
         */  
        public static vertex create_source_vertex(int vertex_id) {  
                vertex v = new vertex(vertex_id, vertex.VERTEX_IS_SOURCE, 0);  
                return v;  
        }  
  
        public int id() {  
                return this.vertex_id;  
        }  
  
        public int predecessor_id() {  
                return this.predecessor_vertex_id;  
        }  
  
        public double distance() {  
                return this.distance;  
        }  
  
        /* 
         * this function returns true if the vertex is 
         * disconnected. disconnected means, having a distance of 
         * Integer.MAX_VALUE 
         */  
        final public boolean is_vertex_disconnected() {  
                return (this.distance == Double.POSITIVE_INFINITY);  
        }  
  
        /* 
         * this function returns true if the vertex is a 
         * source-vertex. a source-vertex has a distance of '0'. 
         */  
        final public boolean is_vertex_source() {  
                return (this.distance == 0);  
        }  
  
        /* 
         * This overrides the default comparator for the priority 
         * queue to the distance(weight) of the corresponding vertex. 
         */  
        @Override  
        public int compareTo (vertex that) {  
                if (this.distance > that.distance)  
                        return 1;  
  
                if (this.distance < that.distance)  
                        return -1;  
  
                return 0;  
        }  
          
        /* 
         * This is overrides the equality check for 2 vertex objects. 
         */  
        @Override  
        public boolean equals (Object obj) {  
                if(obj == this) {  
                        return true;  
                }  
  
                if(obj == null || obj.getClass() != this.getClass()) {  
                        return false;  
                }  
  
                vertex that = (vertex)(obj);  
                return (this.vertex_id == that.vertex_id);  
        }  
  
        /* 
         * stringified representation of a vertex 
         */  
        @Override  
        final public String toString() {  
                return String.format("[id: %d, predecessor-id: %d, distance: %d]",  
                                     this.id(),  
                                     this.predecessor_id(),  
                                     this.distance());  
        }  
}  
  
/* 
 * This class represents an edge in an undirected weighted graph 
 */  
class edge {  
        private int source;  
        private int destination;  
        private double weight;  
  
        public edge (int source, int destination, double weight) {  
                this.source      = source;  
                this.destination = destination;  
                this.weight      = weight;  
        }  
  
        public int src() {  
                return this.source;  
        }  
  
        public int dst() {  
                return this.destination;  
        }  
  
        public double weight() {  
                return this.weight;  
        }  
  
        /* 
         * stringified representation of the graph 
         **/  
        @Override  
        final public String toString() {  
                return String.format("[%d -> %d : %d]",  
                                     this.source, this.destination, this.weight);  
        }  
}  
  
/* 
 * This class represents an undirected weighted graph. An adjacency list is 
 * used to describe the connectivity of the graph. 
 */  
class graph {  
        private Vector<ArrayList<edge>> vertex_list;  
        private int vertices;  
        private int src;  
        private vertex[] path;  
        private PriorityQueue<vertex> pq = new PriorityQueue<vertex>();  
        private String graph_file_name;  
  
        public graph(String file, int path_from) throws IOException {  
                File input_file  = new File(file);  
                Scanner sc       = new Scanner(input_file);  
  
                this.graph_file_name = file;  
                this.vertices        = sc.nextInt();  
                this.vertex_list     = new Vector<ArrayList<edge>>(vertices);  
                this.path            = new vertex[vertices];  
                this.src             = path_from;  
  
                /* 
                 * all elements in this.path are initially disconnected 
                 */  
                for (int i = 0; i < vertices; i++) {  
                        this.path[i] = vertex.create_disconnected_vertex(i);  
                }  
  
                /* 
                 * create array-list of edges for each vertex 
                 */  
                for (int i = 0; i < vertices; i++) {  
                        vertex_list.add(new ArrayList<edge>());  
                }  
  
                /* 
                 * The input of the graph is taken from the text file, and an edge 
                 * is added in the graph with the inputted information. 
                 */  
                while (sc.hasNext()) {  
                        int src    = sc.nextInt();  
                        int dst    = sc.nextInt();  
                        double weight = sc.nextDouble();  
  
                        edge e1 = new edge(src, dst, weight);  
                        edge e2 = new edge(dst, src, weight);  
  
                        this.add_edge(src, e1);  
                        this.add_edge(dst, e2);  
                }  
  
                /* 
                 * find source rooted paths 
                 */  
                this.shortest_path(this.src);  
  
                /* 
                 * dump the current path array 
                 */  
                // System.out.println("*** Final Path ***");  
                // this.dump_path();  
        }  
  
        /* 
         * stringified representation of the graph 
         */  
        @Override  
        final public String toString() {  
                String graph_str = new String();  
  
                for (int v = 0; v < this.vertices; v++) {  
                        String vertex_str = new String();  
                        final ArrayList<edge> edge_list = this.vertex_list.get(v);  
                        final int v_list_size = edge_list.size();  
  
                        // add source vertex  
                        vertex_str = String.format("%4d: ", v);  
  
                        // add all edges except the last one  
                        final int v_list_size_1_less = v_list_size - 1;  
                        for (int i = 0; i < v_list_size_1_less; i++) {  
                                final edge e = edge_list.get(i);  
                                final String edge_string = String.format("%s, ", e);  
  
                                vertex_str = vertex_str + edge_string;  
                        }  
  
                        // add last edge as well.  
                        final String last_edge_string = String.format("%s", edge_list.get(v_list_size_1_less));  
                        vertex_str = vertex_str + last_edge_string;  
  
                        graph_str = graph_str + vertex_str + "\n";  
                }  
  
                return graph_str;  
        }  
  
        /* 
         * this function is called to return the adjacency list for a 
         * given vertex 'v' 
         */  
        public ArrayList<Integer> adj(int v) {  
                ArrayList <edge> edge_list   = this.vertex_list.get(v);  
                final int v_list_size        = edge_list.size();  
                ArrayList <Integer> adj_list = new ArrayList<Integer>(v_list_size);  
  
                for (int i = 0; i < v_list_size; i++) {  
                        edge e = edge_list.get(i);  
  
                        adj_list.add(e.dst());  
                }  
  
                return adj_list;  
        }  
  
        /* 
         * This function is used to add an edge at a corresponding source vertex. 
         */  
        private void add_edge(int v, edge e) {  
                ArrayList<edge> adjacency_list = vertex_list.get(v);  
  
                adjacency_list.add(e);  
        }  
          
        /* 
         * This function computes and displays the path to a destination, 
         * and also displays the distance traversed to reach the destination. 
         */  
        public void path_to(int dst) {  
                // check the basics  
                if (dst < 0 || dst > this.vertices) {  
                        System.out.printf("%d not connected to %d\n", this.src, dst);  
                        return;  
                }  
  
                vertex v = path[dst];  
  
                  
                //handle unconnected vertices                   
                if (v.is_vertex_disconnected()) {  
                        System.out.printf("%d not connected to %d\n", this.src, dst);  
                        return;  
                }  
  
                if (v.is_vertex_source()) {  
                        System.out.printf("%d, distance: %.3f\n", v.id(), v.distance());  
                        return;  
                }  
  
                  
                //populate path stack  
                Stack<Integer> stack        = new Stack<Integer>();  
                final double total_distance = v.distance();  
                int prev_vertex_id          = v.predecessor_id();  
  
                stack.push(v.id());  
                do {  
                        stack.push(prev_vertex_id);  
                        v = this.path[prev_vertex_id];  
                        prev_vertex_id = v.predecessor_id();  
                } while (v.is_vertex_source() == false);  
  
                System.out.printf("Path: ");  
                final int stack_len_1_less = stack.size() - 1;  
                for (int i = 0; i < stack_len_1_less; i++) {  
                        System.out.printf("%d -> ", stack.pop());  
                }  
  
                System.out.printf("%d \n", stack.pop());  
                System.out.printf("The total distance of the path is %.5f \n",total_distance);  
  
                return;  
        }  
          
        /* 
         * Given a source and destination node,this function 
         * returns the distance between the same. 
         */  
        private double get_edge_weight (int src, int dst) {  
                final ArrayList<edge> edge_list = this.vertex_list.get(src);  
  
                for (int i = 0; i < edge_list.size(); i++) {  
                        edge e = edge_list.get(i);  
                        if (e.dst() == dst) {  
                                return e.weight();  
                        }  
                }  
  
                return Double.POSITIVE_INFINITY;  
        }  
  
        /* 
         * this function is called to compute single-source shortest 
         * path in an undirected weighted graph using dijkstra's 
         * algorithm from a source vertex. 
         */  
        public void shortest_path(int src) {  
                /* 
                 * dq_vertices is the list of vertices dequed from the 
                 * priority queue. 
                 */  
                boolean dq_vertices[] = new boolean[this.vertices];  
                vertex sv = vertex.create_source_vertex(src);  
  
                // initialize  
                Arrays.fill(dq_vertices, false);  
                this.pq.add(sv);  
                this.path[sv.id()] = sv;  
  
                /* 
                 * run dijkstra's shortest path algorithm. 
                 */  
                while (this.pq.isEmpty() == false) {  
                        vertex tmp_v = pq.poll();  
                        ArrayList<Integer> adj_list = this.adj(tmp_v.id());  
  
                        dq_vertices[tmp_v.id()] = true;  
  
                        for (int i = 0; i < adj_list.size(); i++) {  
                                vertex tmp_v_neigh = this.path[adj_list.get(i)];  
  
                                if (dq_vertices[tmp_v_neigh.id()] == false) {  
                                        this.relax(tmp_v, tmp_v_neigh);  
                                }  
                        }  
                }  
  
                return;  
        }  
          
        /* 
         * Given two vertices, this function computes a better  
         * path(if any) to a vertex (in this case, v2). 
         */  
        public void relax (vertex v1, vertex v2) {  
                double v1_v2_distance = get_edge_weight(v1.id(), v2.id());  
                double total_distance = (v1.distance() + v1_v2_distance);  
  
                /* 
                 * if total_distance to v2 is less then, we have a 
                 * better path to v2 via v1. 
                 * 
                 * add updated v2 vertex to both the priority queue, 
                 * and the path array. 
                 */  
                if (total_distance <= v2.distance()) {  
                        vertex better_v2  = new vertex(v2.id(), v1.id(), total_distance);  
  
                        // update path array  
                        this.path[better_v2.id()] = better_v2;  
  
                        if (this.pq.contains(v2)) {  
                                this.pq.remove(v2);  
                        }  
                        this.pq.add(better_v2);  
                }  
  
                return;  
        }  
  
        /* 
         * this function is called to display the graph. the graph is 
         * displayed with it's adjacency list representation. 
         * 
         * for example, for a vertex 'v' with edges e1, e2, e3 we have 
         *      'v' : [e1], [e2], [e3] 
         */  
        private void debug_display_edgelist() {  
                for (int v = 0; v < this.vertices; v++) {  
                        ArrayList<edge> adjacency_list = vertex_list.get(v);  
                        final int adj_list_size = adjacency_list.size();  
  
                        System.out.print(v+": ");  
  
                        for (int j = 0; j < adj_list_size - 1; j++) {  
                                System.out.printf ("%s, ", adjacency_list.get(j));  
                        }  
  
                        System.out.printf("%s\n", adjacency_list.get(adj_list_size - 1));  
                }  
  
                return;  
        }  
  
        private void dump_priority_queue() {  
                vertex[] v_list = new vertex[this.pq.size()];  
  
                this.pq.toArray(v_list);  
                Arrays.sort(v_list);  
  
                for (vertex v : v_list) {  
                        System.out.printf("%s\n", v);  
                }  
        }  
  
        private void dump_path() {  
                for (vertex v : this.path) {  
                        System.out.printf("%s\n", v);  
                }  
                System.out.println("----------------------------------------------------------------");  
        }  
  
        /* 
         * this function is called to dump the current graph in a 
         * format suitable for visualizing the graph via graphviz. 
         */  
        final public void dump_graph_for_graphviz() {  
                boolean seen_vertex[] = new boolean[this.vertices];  
  
                Arrays.fill(seen_vertex, false);  
                System.out.printf("graph \"%s\" {\n", this.graph_file_name);  
  
                for (int v = 0; v < this.vertices; v++) {  
                        final ArrayList<edge> edge_list = this.vertex_list.get(v);  
  
                        seen_vertex[v] = true;  
                        for (edge e : edge_list) {  
                                if (seen_vertex[e.dst()])  
                                        continue;  
  
                                System.out.printf("\t%d -- %d [label=\"%.5f\"];\n",  
                                                  e.src(), e.dst(), e.weight());  
                        }  
                }  
  
                System.out.printf("}\n");  
  
                return;  
        }  
}  
  
public class run_graph {  
        public static void main (String args[]) throws IOException {  
                graph test_graph = new graph(args[0], Integer.parseInt(args[1]));  
  
                /* 
                 * show the path 
                 */  
                test_graph.path_to(Integer.parseInt(args[2]));  
        }  
}  

