
# Lưu ý khi sử dụng S3 lifecycle rule 

### Yêu cầu:

Tạo một rule để làm object trong bucket bị delete sau 1 ngày
### Thực trạng:

Hiện tại chưa thể trong 1 ngày đã delete hoàn toàn (lý do: days thực hiện action phải là int từ 1). Tuy nhiên, có thể khiến cho object bị mark as delete, không hiển thị với user nữa sau 1 ngày, đến ngày thứ 2 là xóa hoàn toàn 
### Giải pháp: 
```
{
  "Rules": [
    {
      "Expiration": {
        "Days": 1
      },
      "ID": "delete-expiration-itemư",
      "Filter": {
        "Prefix": "tmp/tmp_001/"
      },
      "Status": "Enabled",
      "NoncurrentVersionExpiration": {
        "NoncurrentDays": 1
      },
      "AbortIncompleteMultipartUpload": {
        "DaysAfterInitiation": 1
      }
    }
  ]
}
```
### Lưu ý: 

1. "Prefix": "tmp/tmp_001/" mà thành viết thành "Prefix": "/tmp/tmp_001/" thì có để năm kia cũng k xóa!

2. Với lifecycle rule này, sẽ cần có 4 ngày để 1 object xóa hoàn toàn (lifecycle lần đầu chạy) vì sẽ cần 48 giờ để lifecycle rule mới tạo valid và chạy. Từ lần chạy thứ 2 thì chỉ cần 2 ngày thôi.

3. Có cần phải enable versioning không: Với rule trên thì đang là có enable versioning.

Tuy nhiên, you can use lifecycle rules for S3 buckets regardless of whether they are enabled for versioning or not.
