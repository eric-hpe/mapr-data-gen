# MapR Data Generator
Data generator for MapR Data Platform

## How to build
```bash
mvn clean compile assembly:single     
```

This should give you `/mapr-data-gen-1.0.jar` in your `target` folder.

## How to run
```bash
./bin/spark-submit --master yarn \ 
--class class com.github.anicolaspp.Generator \ 
mapr-data-gen-1.0.jar [OPTIONS]
```

Current options are: 
```bash  
  usage: parquet-generator
   -C,--compress <arg>                          <String> compression type, valid values are:
                                                uncompressed, snappy, gzip,
                                                lzo (default: uncompressed)
   -f,--format <arg>                            <String> output format type (e.g., parquet (default), maprdb, etc.)
   -o,--output <arg>                            <String> the output file name (default: /ParqGenOutput.parquet)
   -O,--options <arg>                           <str,str> key,value strings that will be passed to the data source of spark in
                                                writing. E.g., for parquet you may want to re-consider parquet.block.size. The
                                                default is 128MB (the HDFS block size).
   -p,--partitions <arg>                        <int> number of output partitions (default: 1)
   -r,--rows <arg>                              <long> total number of rows (default: 10)
   -R,--rangeInt <arg>                          <int> maximum int value, value for any Int column will be generated between
                                                [0,rangeInt), (default: 2147483647)
   -s,--size <arg>                              <int> any variable payload size, string or payload in IntPayload (default: 100)
   -S,--show <arg>                              <int> show <int> number of rows (default: 0, zero means do not show)
   -t,--tasks <arg>                             <int> number of tasks to generate this data (default: 1)
```

An example run would be : 
```bash 
./bin/spark-submit --master yarn \
--class com.github.anicolaspp.Generator mapr-data-gen-1.0.jar  \
-o /user/mapr/tables/test_gen -r 84 -s 42 -p 12
```

This will create `984 ( = 12 * 84)` rows for `case class Data` as 
`[String, Int, Array[Byte], Double, Float, Long, String]` with 42 bytes byte array and 42 chars String, and save this 
as a MapR-DB table in `/user/mapr/tables/test_gen`.

We can all generate parquet data in the following way. 

```bash 
./bin/spark-submit --master yarn \
--class com.github.anicolaspp.Generator mapr-data-gen-1.0.jar  \
-o /user/mapr/tables/test_gen -r 84 -s 42 -p 12 -f /user/mapr/data/parquet
```

In this case the same data is generated and saved in `/user/mapr/data/parquet`
 