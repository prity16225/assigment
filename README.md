#include <iostream>
#include <vector>
using namespace std;

const int MAXN = 100005;

int N, T;
vector<int> graph[MAXN];
int disc[MAXN], low[MAXN];
bool visited[MAXN];
vector<pair<int, int>> bridges;

void tarjan(int u, int parent) {
    visited[u] = true;
    disc[u] = low[u] = ++T;
    for (int v : graph[u]) {
        if (v == parent) continue;
        if (!visited[v]) {
            tarjan(v, u);
            low[u] = min(low[u], low[v]);
            if (low[v] > disc[u]) {
                bridges.push_back({u, v});
            }
        } else {
            low[u] = min(low[u], disc[v]);
        }
    }
}

vector<pair<int, int>> findBridges() {
    T = 0;
    bridges.clear();
    for (int i = 0; i < N; i++) {
        visited[i] = false;
        disc[i] = low[i] = 0;
    }
    for (int i = 0; i < N; i++) {
        if (!visited[i]) {
            tarjan(i, -1);
        }
    }
    return bridges;
}

vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
    N = n;
    for (auto& conn : connections) {
        graph[conn[0]].push_back(conn[1]);
        graph[conn[1]].push_back(conn[0]);
    }
    auto bridges = findBridges();
    vector<vector<int>> critical;
    for (auto& bridge : bridges) {
        critical.push_back({bridge.first, bridge.second});
    }
    return critical;
}

int main() {
    int n;
    cout << "n = ";
    cin >> n;
    string s;
    cout << "connections = ";
    //cin >> s;
    vector<vector<int>> connections;
    cout<<"[";
    for(int i=0;i<n;i++){
        int a,b;
        cin >> a >> b;
        connections.push_back({a,b});
    }
    cout<<"]";
    
    auto critical = criticalConnections(n, connections);
    int i=0;
    cout<<"[";
    for (auto& conn : critical) {
        if(i>0 && i<=critical.size()){cout<<", ";}
        cout << "[" << conn[0] << ", " << conn[1] << "]";
    }
    cout<<"]";
    return 0;
