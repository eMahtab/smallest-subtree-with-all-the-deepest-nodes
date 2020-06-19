# Smallest subtree with all the deepest nodes

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
