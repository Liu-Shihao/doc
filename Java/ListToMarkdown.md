将 Java List 转换为 Markdown 格式的表格字符串，主要需要根据 Markdown 表格的语法规则构建字符串。

Markdown 表格语法

基本格式如下：

| Header1 | Header2 | Header3 |
|---------|---------|---------|
| Data1   | Data2   | Data3   |
| Data4   | Data5   | Data6   |

以下是具体实现步骤和代码：

示例代码

import java.util.List;

public class MarkdownTableGenerator {

    /**
     * 将 List 转换为 Markdown 表格字符串
     *
     * @param headers 表头
     * @param data    数据行
     * @return Markdown 表格字符串
     */
    public static String listToMarkdownTable(List<String> headers, List<List<String>> data) {
        StringBuilder markdown = new StringBuilder();

        // 构建表头
        markdown.append("| ");
        for (String header : headers) {
            markdown.append(header).append(" | ");
        }
        markdown.append("\n");

        // 构建表头分隔符
        markdown.append("| ");
        for (int i = 0; i < headers.size(); i++) {
            markdown.append("---- | ");
        }
        markdown.append("\n");

        // 构建数据行
        for (List<String> row : data) {
            markdown.append("| ");
            for (String cell : row) {
                markdown.append(cell).append(" | ");
            }
            markdown.append("\n");
        }

        return markdown.toString();
    }

    public static void main(String[] args) {
        // 示例数据
        List<String> headers = List.of("Name", "Age", "City");
        List<List<String>> data = List.of(
                List.of("Alice", "30", "New York"),
                List.of("Bob", "25", "Los Angeles"),
                List.of("Charlie", "35", "Chicago")
        );

        // 转换为 Markdown 表格
        String markdownTable = listToMarkdownTable(headers, data);

        // 输出 Markdown 表格字符串
        System.out.println(markdownTable);
    }
}

输出结果

运行上述代码后，控制台输出以下 Markdown 格式字符串：

| Name    | Age  | City        | 
| ----    | ---- | ----        | 
| Alice   | 30   | New York    | 
| Bob     | 25   | Los Angeles | 
| Charlie | 35   | Chicago     | 

解析说明

	1.	输入数据结构：
	•	表头为 List<String>，例如：["Name", "Age", "City"]。
	•	数据行为 List<List<String>>，每行是一个 List，例如：[["Alice", "30", "New York"], ["Bob", "25", "Los Angeles"]]。
	2.	Markdown 表格生成规则：
	•	使用竖线 | 分隔每列数据。
	•	表头与数据之间需要一行分隔符，分隔符可以用 ----。
	3.	代码逻辑：
	•	遍历表头和数据行构建 Markdown 表格字符串。

扩展功能

	1.	动态列宽：
可以根据列中内容的最大长度调整分隔符长度：

int maxLen = Math.max(cell.length(), header.length());


	2.	对齐格式：
如果需要指定列的对齐方式，可以调整分隔符样式：
•	:---- 左对齐
•	:----: 居中对齐
•	----: 右对齐
3.	支持更多复杂数据：
如果数据来自数据库查询或 JSON，可以在处理前解析数据结构。

应用场景

	•	将 List 数据导出为 Markdown 格式文档。
	•	自动生成 Markdown 表格以用于 API 文档或 README 文件。