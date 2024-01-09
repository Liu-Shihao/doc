在 Linux 中，你可以使用 Flink 提供的命令行工具 `flink run` 来提交 Flink 作业。以下是一些基本的步骤：

1. **准备 Flink 作业的可执行 JAR 文件：**
   确保你的 Flink 项目已经被打包成一个可执行的 JAR 文件。你可以使用 Maven 或 Gradle 构建你的项目。

2. **打开终端：**
   打开终端窗口，进入到你的 Flink 安装目录。

3. **使用 `flink run` 命令提交作业：**
   运行以下命令，将你的 Flink 作业提交到 Flink 集群：

   ```bash
   ./bin/flink run -c your.package.YourMainClass /path/to/your/flink-job.jar
   ```

    - `-c your.package.YourMainClass` 指定了你的主类。
    - `/path/to/your/flink-job.jar` 是你构建的 Flink 项目的 JAR 文件的路径。

   例如，如果你的主类是 `com.example.MyFlinkJob`，而 JAR 文件位于 `/path/to/myflinkjob.jar`，命令将是：

   ```bash
   ./bin/flink run -c com.example.MyFlinkJob /path/to/myflinkjob.jar
   ```

4. **查看作业状态：**
   提交成功后，Flink 将输出作业的 JobID。你可以使用以下命令来查看作业的状态：

   ```bash
   ./bin/flink list
   ```

   或者你可以通过 Flink 的 Web UI（默认地址是 http://localhost:8081）来监视作业的状态和性能指标。

请确保 Flink 集群正在运行，并且你的作业的相关依赖已经在集群上可用。你还可以使用 `-m` 选项指定 Flink 集群的地址，例如 `-m yarn-cluster` 或 `-m localhost:8081`。

这是一个简单的命令行提交 Flink 作业的过程。更复杂的配置和调优可以在提交命令中使用其他参数，具体取决于你的需求。可以通过运行 `./bin/flink run --help` 查看更多命令行选项。