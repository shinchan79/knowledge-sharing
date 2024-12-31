#  Glue - Athena: Chỉ định số lượng output file/output file size 
### Câu hỏi 1:

Khi chỉ sử dụng mỗi câu lệnh partition clause, thì data sẽ được chia thành 30 files, mỗi file có dung lượng <1MB. Vì file có dung lượng nhỏ, nên có thể ảnh hưởng đến tốc độ truy vấn. => Làm sao để có thể chỉ định size của file được chia hoặc số file được chia ra?
**Solution 1**: 

Để chỉ định file size hay số lượng output file, có thể sử dụng kỹ thuật "Bucketing" với Athena. Cách thực hiện được hướng dẫn trong bài viết sau:

How can I set the number or size of files when I run a CTAS query in Athena?

Tuy nhiên, với kỹ thuật này thì có một lưu ý: Bucketed table không hỗ trợ INSERT INTO.
### Câu hỏi 2:

Cùng context như trên, nhưng lại muốn sử dụng câu lệnh INSERT INTO thì làm thế nào?
**Solution**:

Sử dụng Glue repartition:

Các bước thực hiện tham khảo bài viết sau:

[Build a Data Lake Foundation with AWS Glue and Amazon S3]
https://aws.amazon.com/blogs/big-data/build-a-data-lake-foundation-with-aws-glue-and-amazon-s3/

Lưu ý rằng, tại bước "13.View the job." trong bài viết, cần add thêm đoạn code:

```
datasource_df = dropnullfields3.repartition(<số file output bạn mong muốn>)
```

vào sau dòng
```
dropnullfields3 = DropNullFields.apply(frame = resolvechoice2, transformation_ctx = "dropnullfields3")
```

và sửa lại
```
datasink4 = glueContext.write_dynamic_frame.from_options(frame = dropnullfields3, connection_type = "s3", connection_options = {"path": "<your_s3_path>"}, format = "parquet", transformation_ctx = "datasink4")
```

thành
```
datasink4 = glueContext.write_dynamic_frame.from_options(frame = datasource_df, connection_type = "s3", connection_options = {"path": "<your_s3_path>"}, format = "parquet", transformation_ctx = "datasink4")
```
Tham khảo thêm về repartition tại đây:

https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-samples-legislators.html

Query với Athena:

    Tạo bảng:
```
CREATE EXTERNAL TABLE IF NOT EXISTS aaaa (
  dispatching_base_num string,
  pickup_date string,
  locationid bigint)
STORED AS PARQUET
LOCATION 's3://athena-examples/parquet/'
tblproperties ("parquet.compress"="SNAPPY");
```
    Insert thử:
```
insert into aaaa ("dispatching_base_num", "pickup_date", "locationid") values ('aa', 'vvv', 123);
```
Lưu ý: 

### Câu hỏi 3:

Ngoài Bucketing thì có cách nào khác để dùng Athena chỉ định số output file không?
**Solution 3:**

Bucketing is the only way!
