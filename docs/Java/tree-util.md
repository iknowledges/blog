# 一个树结构的工具类

## 树结构借口
```java
public interface ITreeNode<T extends ITreeNode<T>> {
    String getId();

    void setId(String id);

    String getPid();

    void setPid(String pid);

    List<T> getChildren();

    void setChildren(List<T> children);
}
```

## 工具类
```java
public class TreeNodeUtil {

    public static <T extends ITreeNode<T>> List<T> getParent(List<T> treeList) {
        return treeList.stream().filter(t -> !StringUtils.hasText(t.getPid())).collect(Collectors.toList());
    }

    public static <T extends ITreeNode<T>> List<T> getChildNode(T tree, List<T> treeList) {
        return treeList.stream().filter(t -> StringUtils.hasText(t.getPid()) && tree.getId().equals(t.getPid())).collect(Collectors.toList());
    }

    public static <T extends ITreeNode<T>>  void listToTree(T tree, List<T> treeList) {
        List<T> childNode = getChildNode(tree, treeList);
        if (childNode.size() == 0)
            return;
        tree.setChildren(childNode);
        for (T t : childNode) {
            listToTree(t, treeList);
        }
    }

    public static <T extends ITreeNode<T>> List<T> getMultiTree(List<T> treeList) {
        List<T> res = new ArrayList<>();
        List<T> parent = getParent(treeList);
        if (parent.size() == 0) {
            return res;
        }
        for (T pTree : parent) {
            listToTree(pTree, treeList);
            res.add(pTree);
        }
        return res;
    }

    public static <T extends ITreeNode<T>> List<T> getNodeList(T tree) {
        List<T> res = new ArrayList<>();
        treeToList(tree, res);
        return res;
    }

    public static <T extends ITreeNode<T>> void treeToList(T tree, List<T> treeList) {
        if (tree == null) {
            return;
        }
        treeList.add(tree);
        List<T> children = tree.getChildren();
        if (CollectionUtil.isEmpty(children)) {
            return;
        }
        for (T child : children) {
            treeToList(child, treeList);
        }
    }
}
```