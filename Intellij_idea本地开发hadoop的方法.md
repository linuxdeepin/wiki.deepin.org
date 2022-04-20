##### 环境
+ 我的intellij idea版本是2017.2.2，hadoop版本2.7.4，操作系统Deepin15.4.1，使用《hadoop权威指南》的天气统计源码作为示例。
##### 参考
+ 文章参考：http://blog.csdn.net/programmer_wei/article/details/45286749
## 步骤
### 安装Java环境
+ 安装好Java环境，我的Java环境为jdk1.8_144（安装过程不赘述）
### 下载
+ 在官网(http://hadoop.apache.org/releases.html)上下载hadoop，不需要进行环境变量的配置，下载解压到相应目录即可
### 新建项目
+ 打开intellij idea创建一个空项目，并给空项目命名（我命名为：Temperature），完成之后弹出 Project Structure 设置对话框
### 添加Modules
 + 首先选择左侧的【Modules】，中间点击【+】号，选【Add Module】，之后【Next】，给Module取个名字（TemperatureModule），【Finish】
+  之后在右侧，【Sources】下，【Language level】选择8（因为我安装的是Java1.8）；
### 添加Hadoop依赖
 + 这是重点，【Dependencies】下，添加你加压出来的Hadoop的jar包：
 ++（1）点击右侧的【+】，选择第一项（1 JARS or directories）
++（2）添加~/softwares/hadoop-2.7.4/share/hadoop/common，~/softwares/hadoop-2.7.4/share/hadoop/common/lib，~/softwares/hadoop-2.7.4/share/hadoop/hdfs，~/softwares/hadoop-2.7.4/share/hadoop/yarn，~/softwares/hadoop-2.7.4/share/hadoop/mapreduce；
++（3）点击【Apply】
### 添加Artifacts
+ 选择左侧的【Artifacts】-> 【+】-> 【JAR】->【Empty】，取个名字（maxTemperature），在【output layout】下，点击带倒三角符号【+】，选择【Module output】，选择我们之前创建的Module，【OK】即可；
### 创建Mainifest（可选）
+ 点击【Apply】，会出现一个对话框，点击【create Mainifest】，选择Main 方法所在的类，点击【OK】
### 添加类文件
+ 在左侧的src目录下，添加下面的几个Java类
### 源码
<code>
// MaxTemperatureMapper类：</br>
 import java.io.IOException;</br>
import org.apache.hadoop.io.IntWritable;</br>
import org.apache.hadoop.io.LongWritable;</br>
import org.apache.hadoop.io.Text;</br>
import org.apache.hadoop.mapreduce.Mapper;</br>
</br>
public class MaxTemperatureMapper</br>
        extends Mapper<LongWritable, Text, Text, IntWritable> {  </br>
    private static final int MISSING = 9999;</br>
    @Override</br>
    public void map(LongWritable key, Text value, Context context)</br>
            throws IOException, InterruptedException {</br>
        String line = value.toString();</br>
        String year = line.substring(15, 19);</br>
        int airTemperature;</br>
        if (line.charAt(87) == '+') { // parseInt doesn't like leading plus signs</br>
            airTemperature = Integer.parseInt(line.substring(88, 92));</br>
        } else {</br>
            airTemperature = Integer.parseInt(line.substring(87, 92));</br>
        }</br>
        String quality = line.substring(92, 93);</br>
        if (airTemperature != MISSING && quality.matches("[01459]")) {</br>
            context.write(new Text(year), new IntWritable(airTemperature));</br>
        }</br>
    }</br>
}</br>
</br>
// MaxTemperatureReducer类：</br>
</br>
import java.io.IOException;</br>
import org.apache.hadoop.io.IntWritable;</br>
import org.apache.hadoop.io.Text;</br>
import org.apache.hadoop.mapreduce.Reducer;</br>
</br>
public class MaxTemperatureReducer</br>
        extends Reducer<Text, IntWritable, Text, IntWritable> {</br>
    @Override</br>
    public void reduce(Text key, Iterable<IntWritable> values,Context context) throws IOException, InterruptedException {</br>
        int maxValue = Integer.MIN_VALUE;</br>
        for (IntWritable value : values) {</br>
            maxValue = Math.max(maxValue, value.get());</br>
        }</br>
        context.write(key, new IntWritable(maxValue));</br>
    }</br>
}</br>
//MaxTemperature类：</br>
import org.apache.hadoop.fs.Path;</br>
import org.apache.hadoop.io.IntWritable;</br>
import org.apache.hadoop.io.Text;</br>
import org.apache.hadoop.mapreduce.Job;</br>
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;</br>
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;</br>
</br>
public class MaxTemperature {</br>
    public static void main(String[] args) throws Exception {</br>
        if (args.length != 2) {</br>
            System.err.println("Usage: MaxTemperature <input path\> <output path\>");</br>
            System.exit(-1);</br>
        }</br>
        Job job = new Job();</br>
        job.setJarByClass(MaxTemperature.class);</br>
        job.setJobName("Max temperature");</br>
</br>
        FileInputFormat.addInputPath(job, new Path(args[0]));</br>
        FileOutputFormat.setOutputPath(job, new Path(args[1]));</br>
</br>
        job.setMapperClass(MaxTemperatureMapper.class);</br>
        job.setReducerClass(MaxTemperatureReducer.class);</br>
</br>
        job.setOutputKeyClass(Text.class);              </br>
        job.setOutputValueClass(IntWritable.class);</br>
</br>
        System.exit(job.waitForCompletion(true) ? 0 : 1);</br>
    }</br>
}</br>
</code>
### 编辑配置文件
+ 编辑运行配置文件【Edit configurations】，在左侧，点击【+】，选择【Application】，在右侧，给配置文件取个名字（maxTemperature），下面选择Main方法所在的类，【Program arguments】填如输入数据的路径，和输出数据的路径（输出数据路径为自动创建，不能提前手动创建）；
### 运行
+ 点击【Run】，等待运行结果，到你填写的输出路径查看运行结果