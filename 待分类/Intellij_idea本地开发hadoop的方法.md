---
title: 待分类/Intellij_idea本地开发hadoop的方法
description: 
published: true
date: 2022-05-07T07:47:22.598Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:36:33.921Z
---

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
+ 之后在右侧，【Sources】下，【Language level】选择8（因为我安装的是Java1.8）；

### 添加Hadoop依赖

+ 这是重点，【Dependencies】下，添加你加压出来的Hadoop的jar包：
	1. 点击右侧的【+】，选择第一项（1 JARS or directories）
	2. 添加\~/softwares/hadoop-2.7.4/share/hadoop/common，\~/softwares/hadoop-2.7.4/share/hadoop/common/lib，\~/softwares/hadoop-2.7.4/share/hadoop/hdfs，\~/softwares/hadoop-2.7.4/share/hadoop/yarn，\~/softwares/hadoop-2.7.4/share/hadoop/mapreduce；
	3. 点击【Apply】

### 添加Artifacts

+ 选择左侧的【Artifacts】-> 【+】-> 【JAR】->【Empty】，取个名字（maxTemperature），在【output layout】下，点击带倒三角符号【+】，选择【Module output】，选择我们之前创建的Module，【OK】即可；

### 创建Mainifest（可选）

+ 点击【Apply】，会出现一个对话框，点击【create Mainifest】，选择Main 方法所在的类，点击【OK】

### 添加类文件

+ 在左侧的src目录下，添加下面的几个Java类

### 源码
```java
// MaxTemperatureMapper类：
import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

public class MaxTemperatureMapper
        extends Mapper<LongWritable, Text, Text, IntWritable> {  
    private static final int MISSING = 9999;
    @Override
    public void map(LongWritable key, Text value, Context context)
            throws IOException, InterruptedException {
        String line = value.toString();
        String year = line.substring(15, 19);
        int airTemperature;
        if (line.charAt(87) == '+') { // parseInt doesn't like leading plus signs
            airTemperature = Integer.parseInt(line.substring(88, 92));
        } else {
            airTemperature = Integer.parseInt(line.substring(87, 92));
        }
        String quality = line.substring(92, 93);
        if (airTemperature != MISSING && quality.matches("[01459]")) {
            context.write(new Text(year), new IntWritable(airTemperature));
        }
    }
}

// MaxTemperatureReducer类：

import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

public class MaxTemperatureReducer
        extends Reducer<Text, IntWritable, Text, IntWritable> {
    @Override
    public void reduce(Text key, Iterable<IntWritable> values,Context context) throws IOException, InterruptedException {
        int maxValue = Integer.MIN_VALUE;
        for (IntWritable value : values) {
            maxValue = Math.max(maxValue, value.get());
        }
        context.write(key, new IntWritable(maxValue));
    }
}
//MaxTemperature类：
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class MaxTemperature {
    public static void main(String[] args) throws Exception {
        if (args.length != 2) {
            System.err.println("Usage: MaxTemperature <input path\> <output path\>");
            System.exit(-1);
        }
        Job job = new Job();
        job.setJarByClass(MaxTemperature.class);
        job.setJobName("Max temperature");

        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));

        job.setMapperClass(MaxTemperatureMapper.class);
        job.setReducerClass(MaxTemperatureReducer.class);

        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);

        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

### 编辑配置文件

+ 编辑运行配置文件【Edit configurations】，在左侧，点击【+】，选择【Application】，在右侧，给配置文件取个名字（maxTemperature），下面选择Main方法所在的类，【Program arguments】填如输入数据的路径，和输出数据的路径（输出数据路径为自动创建，不能提前手动创建）；

### 运行

+ 点击【Run】，等待运行结果，到你填写的输出路径查看运行结果
