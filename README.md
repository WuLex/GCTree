# GCTree
JVM 垃圾收集技术研究

根搜索算法

![](https://i.imgur.com/C1BT8MT.png)

<pre>
对象存活判断
         1）引用计数法
            每个对象有一个引用的计数属性，新增一个引用时计数加1，引用释放的时候计数减一，
         计数为0的时候可以回收，此方法简单，但是无法解决对象的相互循环引用的问题。

         2）根搜索算法
            也叫可达性分析算法。
            从GC Roots开始向下搜索，搜索所走过的路径称为引用链。当一个对象到GC Roots没有
         任何引用链时，则证明这个对象是不可用的。不可达对象。

            在Java中，GC Roots包括：
                                  1）虚拟机栈中引用的对象。
                                  2）方法区中类静态属性实体引用的对象。
                                  3）方法区中常量引用的对象。
                                  5）本地方法栈中JNI引用的对象。
</pre>

<pre>
无论通过引用计数法判断对象的引用数量，还是通过根搜索算法判断对象的引用链是否可达，判断对象
是否存活都与“引用”有关。

      Java中将引用分为
           1）强引用
              就是在程序代码中普遍存在的，例如Object obj = new Object();
              只要强引用还在，垃圾收集器就永远不会回收。

           2）软引用
              软引用用来描述一些还有用，但并不是必须的对象。对于软引用关联着的对象，在系统
           将要发生内存溢出之前，将会把这些对象列进回收范围之中并进行第二次回收，如果这次
           回收还是没有足够的内存，才会跑出内存溢出。

           3）弱引用
              弱引用也是用来描述非必须对象的，但是它的强度比弱引用更弱一些，被弱引用关联
           的对象只能生存到下一次垃圾收集发生之前，当垃圾收集器工作时，无论当前内存是否
           足够，都会回收掉弱引用关联的对象。

           5）虚引用
              虚引用也称为幽灵引用，它是最弱的一种引用，一个对象是否有虚引用的存在，完全不
           会对其生存环境构成影响，为一个对象设置虚引用关联的唯一目的就是希望能在这个对象
           被收集器回收的时候收到一个系统通知。
</pre>

##垃圾收集算法

![](https://i.imgur.com/VFK5hV7.png)

<pre>
标记清除算法

     优点：基于最基础的可达性分析算法，它是最基础的收集算法。
     缺点：
         1）效率问题
            标记和清除两个过程的效率都不高。
         2）空间问题
            这会导致分配大内存对象时，无法找到足够的连续内存，从而提前触发一次垃圾收集。
</pre>

![](https://i.imgur.com/46xOKbf.png)

<pre>
标记复制算法

     思路：
          1）把内存划分为大小相等的两块，每次只使用其中一块。
          2）当一块内存用完了，就将还存货的额对象复制到另一块上，而后使用这一块。
          3）再把已使用过的那块内存空间一次清理掉，而后重复步骤2

     优点：
         1）这使得每次都是只对半个区进行内存回收。
         2）内存分配时不用考虑内存碎片问题。
         3）实现简单，运行高效。

     缺点：
         1）空间浪费。
         2）效率随对象存活率升高而变低
            当对象存活率较高时，需要进行较多的复制操作，效率将会变低。
</pre>

![](https://i.imgur.com/U5XM4V2.png)

<pre>
标记整理算法

      步骤：
          1）标记阶段
             标记阶段与标记清除算法一样。
          2）整理
             但后续不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动；
             然后直接清理掉端边界意外的内存。

      优点：
          1）效率随存活对象升高而降低。
          2）不会产生内存碎片。   
</pre>

<pre>
分代收集算法

      算法思路：
 
              根据对象存活周期的不同将内存划分成几块；
              这样可以根据各个年代的特点进行最适当的手机算法
    
      1）新生代：
         每次垃圾收集都有大批对象死去，只有少量存活。
         采用标记复制算法。

      2）老年代：
         对象存活率高，没有额外的空间可以分配担保
         采用标记清理算法或者标记整理算法。
</pre>

##垃圾收集器

<pre>
Serial 串行收集器
</pre>

<pre>
ParNew 并行收集器
</pre>

<pre>
Parallel Scavenge 并行清除收集器
</pre>

<pre>
Serial Old收集器
</pre>

<pre>
Parallel Old收集器
</pre>

<pre>
CMS收集器
</pre>

<pre>
G1 Garbage First收集器
</pre>

<pre>
JVM内存分配策略
</pre>
