1.节点的定义Node, id唯一标识一个图中的点；name随便写，方便辨别；type比较重要，与处理代码对应；detail是这个节点
算法的配置信息。 其他的数据都是前端展示用到的数据。  
```java
    private int id;
    private String name;
    private String type;

    private List<Integer> in_ports;
    private List<Integer> out_ports;

    private int pos_x;
    private int pos_y;

    private String iconClassName;

    private Map<String, Object> detail;   

```
2.边的定义Edge。
```java
    private int id;
    private int src_node_id;
    private int src_output_idx;

    private int dst_node_id;
    private int dst_input_idx;


```

3.前后端交互图的定义DAG
```java
    @Id
    private String id;

    @NotNull
    private String name;

    @JsonFormat(locale = "zh", timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
    private Date createTime;

    @JsonFormat(locale = "zh", timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
    private Date updateTime;
    private List<Node> nodes;
    private List<Edge> edges;


```

4.抽象父类ComputeUnit，ckecking 方法供调用者检查数据输入是否类型正确，err方法返回报错信息，compute方法做正常的计算；
```java
public abstract class ComputeUnit {
    protected Object inputData; //corpus,ldadataset,model...

    public abstract boolean checking();

    public abstract Object compute();

    public abstract String err();

}
```


5.简单工厂ComputeUnitFactory,根据DAG图中的node，创建对应的执行单元
```
public ComputeUnit getComputeUnit(Node node, Object input){}

```

6.DAG图的调度

6.1.安全检查：是否环路，是否连通，每个节点的配置是否标准，每个节点运行动态接受的数据是否是对应的数据结构。


6.2.划分阶段，比如图1->2,1->3,2->4,3->4; 就可以分成3个阶段，第一阶段1，第二阶段23，第三阶段4。同一阶段可以并行。

6.3 dag图的算法，在DAGBuilder类中实现，图的存储用了邻接列表的方式。

7.DAG图的执行器DAGRunner
```java
public Object run_threads() throws InterruptedException {
        ComputeUnitFactory computeUnitFactory = new ComputeUnitFactory();
        
        Map<Integer, Object> store = new HashMap<>();
        List<List<Integer>> stages = dagBuilder.getStages();
        
        for (List<Integer> stage : stages) {
                Thread[] threads = new  Thread[stage.size()] ;
                
            for(int i= 0;i<stage.size();i++){
                Node node   = dag.getNodeByID( stage.get(i) );
                threads[i] =  new Thread(  new ComputeUnitThread(node,dagBuilder,store))    ;
                threads[i].start();
            }
            for(int i=0;i<stage.size();i++){
                threads[i].join();
            }
            Set<Integer> preStageNodes = new HashSet<>();
            for (Integer id : stage) {
                preStageNodes.addAll(dagBuilder.parentNodes(id));
            }
            for (Integer id : preStageNodes) {
                store.remove(id);
            }
        }

        return store.values().toArray()[0];
    }

```
