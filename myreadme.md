## SLAM in Autonomous Driving book (SAD book)

* 更改适配Ubuntu18.04、gcc 7.5 g++ 7.5
* 添加ros topic交互接口
* 各个章节分功能独立工程化，能独立使用
## 总体更改

1. 取消并发
   注释：// #include <execution>
   std::for_each(std::execution::par_unseq, model_.begin(), model_.end(), [&](const Model2DPoint& pt)
   ...
   )
   改为：
   for (const Model2DPoint& pt : model_)
   {
   ...
   }
2. error: conflicting declaration ‘typedef struct LZ4_streamDecode_t LZ4_streamDecode_t’

* 原因：当安装ros的时候，你会获得两个独立并且不兼容的LZ4版本，一个用于ros序列化（serialization），一个用于pcl中flann里的kd树
* 参考：https://blog.csdn.net/billowwong/article/details/122324083?utm_medium=distribute.pc_relevant_t0.none-task-blog-2~default~CTRLIST~Rate-1-122324083-blog-119419078.pc_relevant_aa_2&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2~default~CTRLIST~Rate-1-122324083-blog-119419078.pc_relevant_aa_2&utm_relevant_index=1
* 这里使用第二种方案：
  sudo mv /usr/include/flann/ext/lz4.h /usr/include/flann/ext/lz4.h.bak
  sudo mv /usr/include/flann/ext/lz4hc.h /usr/include/flann/ext/lz4.h.bak

sudo ln -s /usr/include/lz4.h /usr/include/flann/ext/lz4.h
sudo ln -s /usr/include/lz4hc.h /usr/include/flann/ext/lz4hc.h

3. slam_in_autonomous_driving/src/ch7/icp_3d.cc:77:21: error: return-statement with no value, in function returning ‘bool’ [-fpermissive]
   return;
4. 需要安装velodyne_msgs
5. 需要编译g2o及pongolin，不需要安装

### 第一章
