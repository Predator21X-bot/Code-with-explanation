# 🧩 Keys and Rooms

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/keys-and-rooms/](https://leetcode.com/problems/keys-and-rooms/)

---

## 🧠 Problem Understanding

We are given:

* `n` rooms labeled `0 → n-1`
* Each room contains keys to other rooms

👉 Initially:

* We can only enter **room 0**

👉 Goal:

* Determine if we can visit **all rooms**

---

## 🔍 Key Observations

* Rooms → nodes
* Keys → connections (edges)
* This is a **graph traversal problem**

---

## 🧠 Core Idea

1. Start from room `0`
2. Use DFS to visit all reachable rooms
3. Track visited rooms
4. Check if all rooms are visited

---

## 🧪 Example

```cpp id="exk1"
Input:
rooms = [[1],[2],[3],[]]

Output:
true
```

---

## 🐢 Brute Force Idea

* Try all paths recursively without tracking visited

### ❌ Problem:

* Infinite loops
* Repeated work

---

## ⚡ Optimized Approach (DFS)

### 💡 Idea

* Use DFS to explore graph
* Use `visited` array to avoid revisiting

---

## 💻 Code

```cpp id="dfscode"
class Solution {
public:
    void dfs(int currentRoom, vector<vector<int>>& rooms, vector<bool>& visited){
        if(visited[currentRoom]) return;

        visited[currentRoom] = true;

        for(int key : rooms[currentRoom]){
            dfs(key, rooms, visited);
        }
    }

    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int n = rooms.size();
        vector<bool> visited(n, false);

        dfs(0, rooms, visited);

        for(int i = 0; i < n; i++){
            if(!visited[i]) return false;
        }

        return true;
    }
};
```

---

## ⏱ Complexity

* Time: **O(V + E)**
* Space: **O(V)**

---

## ⚠️ Edge Cases

* Only one room
* No keys at all
* Cyclic connections

---

## ❗ Common Mistakes (VERY IMPORTANT)

* Forgetting `visited` → infinite recursion
* Calling DFS on same node repeatedly
* Returning inside loop prematurely
* Confusing:

  * `rooms[i]` → list of keys
  * `key` → actual next room

---

## 🧠 Patterns Learned

* Graph traversal using DFS
* Reachability problems
* Use of visited array

---

## 🏷 Tags

* Graph
* DFS
* Recursion

---

## 🧠 When to Use This

* When problem involves:

  * nodes + connections
  * “can we reach all nodes?”
* Classic graph traversal scenario

---

## 🔥 Mental Model (Revision Key)

> “Start from 0 → DFS → mark visited → check all visited”

---

## 🚀 Takeaway

* Convert problem → graph
* Use DFS/BFS for traversal
* Always track visited nodes
