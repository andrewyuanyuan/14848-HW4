# 14848-HW4
This is my repository for HW-4: Running Hadoop MapReduce on Google Cloud Platform

## Steps

1. Create a Dataproc cluster, upload the source-code folder into Cloud Storage staging bucket.

![1](https://raw.githubusercontent.com/andrewyuanyuan/14848-HW4/main/screenshots/1.png)

![2](https://raw.githubusercontent.com/andrewyuanyuan/14848-HW4/main/screenshots/2.png)

2. Find the bucket URL with `gsutil ls` comand, and copy that id. Use command `gsutil ls
   gs://dataproc-staging-us-west2-479726671653-nd21geni/source-code` to see the content in this bucket, make sure we have all the needed files uploaded in this bucket.

   ![3](https://raw.githubusercontent.com/andrewyuanyuan/14848-HW4/main/screenshots/3.png)

3. Use command `gsutil cp -r gs://dataproc-staging-us-west2-479726671653-nd21genj/source-code/ .` to copy all needed files from my GCP Buckets to my local cluster.

   ![4](https://raw.githubusercontent.com/andrewyuanyuan/14848-HW4/main/screenshots/4.png)

4. Use command `hadoop fs -put data/ /` to copy data files from local cluster to HDFS, and use command `hadoop fs -ls /data` to make sure the copy operation is successfully.

   ![5](https://raw.githubusercontent.com/andrewyuanyuan/14848-HW4/main/screenshots/5.png)

5. Execute MapReduce with the following commands

   ```bash
   hadoop jar /usr/lib/hadoop/hadoop-streaming.jar -files temperature_mapper.py,temperature_reducer.py -input /data/* -output /temperatureResultsOutputFolder -mapper 'python temperature_mapper.py' -combiner 'python temperature_reducer.py' -reducer 'python temperature_reducer.py'
   ```

6. The execution result is shown in the screenshot below:		![6](https://raw.githubusercontent.com/andrewyuanyuan/14848-HW4/main/screenshots/6.png)

7. Use command `hadoop fs -ls/temperatureResultsOutputFolder` to check the output folder, where contains all the result files, the number should equals to the reducer number.

   ![7](https://raw.githubusercontent.com/andrewyuanyuan/14848-HW4/main/screenshots/7.png)

8. Use command `hadoop fs -getmerge /temperatureResultsOutputFolder temperatureResults` to merge the output files together and copy the final result file to my GCP Buckets using command `gsutil cp temperatureResults gs://dataproc-staging-us-west2-479726671653-nd21genj/`

   ![8](https://raw.githubusercontent.com/andrewyuanyuan/14848-HW4/main/screenshots/8.png)

9. We can download the final result file from the GCP Buckets now, you can also view this file `temperatureResults` in this repository.

   ![9](https://raw.githubusercontent.com/andrewyuanyuan/14848-HW4/main/screenshots/9.png)

