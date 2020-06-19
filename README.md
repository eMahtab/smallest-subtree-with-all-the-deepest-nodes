# Smallest subtree with all the deepest nodes
## https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes

Given a binary tree rooted at root, the depth of each node is the shortest distance to the root.

A node is deepest if it has the largest depth possible among any node in the entire tree.

The subtree of a node is that node, plus the set of all descendants of that node.

Return the node with the largest depth such that it contains all the deepest nodes in its subtree.

**Note:**

1. The number of nodes in the tree will be between 1 and 500.
2. The values of each node are unique.

😳 😳 This is exact same question as https://github.com/eMahtab/lowest-common-ancestor-of-deepest-leaves, but framed differently.

## Implementation : Iterative
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode subtreeWithAllDeepest(TreeNode root) {
        if(root == null)
            return root;
        
        Map<TreeNode, TreeNode> parentMap = new HashMap<>();
        parentMap.put(root, null);
        Queue<TreeNode> q = new ArrayDeque<>();
        q.add(root);
        Queue<TreeNode> deepestLevelNodes = null;
        // First Pass : Level order traversal using BFS
        // After first pass, deepestLevelNodes queue will have leaf nodes at the deepest level
        while(!q.isEmpty()) {
            deepestLevelNodes = new ArrayDeque<>(q); 
            int size = q.size();
            for(int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                if(node.left != null) {
                   parentMap.put(node.left, node); 
                   q.add(node.left);   
                }
                if(node.right != null) {
                    parentMap.put(node.right, node);
                    q.add(node.right);
                }
            }
        }
        
        // Second Pass : We move upwards in the tree, from nodes at the deepest level to root node.
        // To move upwards, we add the parent of the node in deepestLevelNodes queue.
        // We keep doing this untill there is only one TreeNode in deepestLevelNodes queue.
        while(deepestLevelNodes.size() > 1) {
            Set<TreeNode> set = new HashSet<>();
            int size = deepestLevelNodes.size();
            for(int i = 0; i < size; i++) {
                set.add(parentMap.get(deepestLevelNodes.poll()));
            }
            for(TreeNode node : set) 
                deepestLevelNodes.add(node);
        }
        
        return deepestLevelNodes.poll();
        
    }
}
```

#References :
https://www.youtube.com/watch?v=QF7ZBH8mXHE
