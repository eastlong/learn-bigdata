# 流处理API
## Source
### 从集合中读取数据

<details>
<summary>从集合中读取数据</summary>

```scala
package com.atiguigu.source

import java.util.Properties

import org.apache.flink.api.scala._
import org.apache.flink.api.common.serialization.SimpleStringSchema
import org.apache.flink.streaming.api.scala.StreamExecutionEnvironment

object Sensor {
  case class SensorReading(id: String, timestamp: Long, temperature: Double)

  def main(args: Array[String]): Unit = {
    val env = StreamExecutionEnvironment.getExecutionEnvironment
    val stream1 = env.fromCollection(List(
      SensorReading("sensor_1", 1547718199, 35.80018327300259),
      SensorReading("sensor_6", 1547718201, 15.402984393403084),
      SensorReading("sensor_7", 1547718202, 6.720945201171228),
      SensorReading("sensor_10", 1547718205, 38.101067604893444)
    ))
    println("### start ###")
    stream1.print("stream1:").setParallelism(1)
    env.execute("Sensor")
  }
}

```
</details>


### 从文件中读取数据
<details>
<summary>从文件中读取数据</summary>

```scala
package com.atiguigu.source

import java.util.Properties

import org.apache.flink.api.scala._
import org.apache.flink.api.common.serialization.SimpleStringSchema
import org.apache.flink.streaming.api.scala.StreamExecutionEnvironment

object Sensor {
  case class SensorReading(id: String, timestamp: Long, temperature: Double)

  def main(args: Array[String]): Unit = {
    val env = StreamExecutionEnvironment.getExecutionEnvironment

    val fileName = this.getClass().getClassLoader().getResource("sensor.txt").getPath();//获取文件路径
    val stream2 = env.readTextFile(fileName)

    println("### start ###")
    stream2.print("stream2:").setParallelism(1)
    env.execute("Sensor")
  }
}
```
</details>

### 以kafka消息队列的数据作为来源




