class Solution {
public:
    void dfs(vector<pair<int,double>> adj[],bool *visited,int node){
           visited[node]=1;
        for(auto k:adj[node]){
            if(!visited[k.first])
                dfs(adj,visited,k.first);
                }
    }
    
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
        // first step construct an adjacency list 
        vector<pair<int,double>> adj[n];
        for(int i=0;i<edges.size();i++){
               int u=edges[i][0];
               int v=edges[i][1];
               double w=succProb[i];
              adj[u].push_back({v,w});
              adj[v].push_back({u,w});
         }
  // second step is to see if start and end are connected or not 
    // for this we will do dfs from start 
   // if end is not visited in that dfs call that means they are not connected
        // hence return 0
        bool visited[n];
        memset(visited,0,n);
        dfs(adj,visited,start);
        if(visited[end]==0)
            return 0;
        
        
        
        //We have reached here that means start and end are connected
        // Now we will modify djkstra algorithm
        // In djkstra we have to find min distance but here we have to find max distance 
        //  So we will use a max heap;
        memset(visited,0,n);
        double key[n]; 
      for(int i=0;i<n;i++){
                key[i]=0;
        
        }
        priority_queue<pair<double,int>>pq;
        pq.push({1.0,start});
              key[start]=1.0; // initializing it from 1 because we have to do multiplication
        for(int i=0;i<n;i++){
            int u=pq.top().second;
            pq.pop();
            visited[u]=1;
            
            for(auto k:adj[u]){
                 int v=k.first;
                double w=k.second;
              
                if(!visited[v]&&w*key[u]>key[v]){
                        key[v]=key[u]*w;
                      pq.push({key[v],v});
                    }
                      }
        }
        
        return key[end];
        
        
        
        
    }
};