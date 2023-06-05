---
title: 获取树形结构的路径
date: 2023-06-06 10:27:27
categories: [work]
tags: [tips]
---

## 代码

```javascript
// 查找节点路径的函数
function findNodePath(root, target) {
  const path = []; // 存储路径的数组

  // 递归辅助函数
  function dfs(node, target) {
    if (node.id == target) {
      path.push(node); // 找到目标节点，将其值添加到路径中
      return true;
    }
    if (node.children) {
      for (let child of node.children) {
        if (dfs(child, target)) {
          path.unshift(node); // 找到目标节点的子节点，将当前节点的值添加到路径的开头
          return true;
        }
      }
    }

    return false;
  }

  // 对象
  dfs(root, target); // 调用递归函数开始查找
  // 数组
  for (let node of tree) {
    if (dfs(node, targetId, [])) {
      break;
    }
  }
  return path;
}

// 查找节点路径的函数
function findNodePath(tree, targetId) {
  const path = []; // 存储路径的数组

  // 递归辅助函数
  function dfs(node, targetId, currentPath) {
    if (node.id == targetId) {
      currentPath.push(node); // 找到目标节点，将其id添加到当前路径中
      path.push(...currentPath); // 将当前路径添加到路径数组中
      return true;
    }

    for (let child of node.children || []) {
      const newPath = [...currentPath];
      newPath.push(node); // 将当前节点的id添加到当前路径中
      if (dfs(child, targetId, newPath)) {
        return true;
      }
    }

    return false;
  }

  for (let node of tree) {
    if (dfs(node, targetId, [])) {
      break;
    }
  }

  return path;
}
```