============================**Tiếng Việt**================================

# Ý nghĩa các thông số cache:
- Default TTL sẽ chỉ valid nếu như object không chứa Cache-Control hoặc Expires header. => trường hợp này thời gian cache sẽ là 86400 giây (~24 giờ). Trường hợp có header và caching time được set trong header nằm trong khoảng minimum-maximum TTL (1 - 31536000 trong case này), thì caching time header sẽ overwrite Default, lúc này TTL = TTL được set
- Nếu object chứa 1 trong 2 header, nhưng cache time lại nhỏ hơn Minimum TTL (1s trong case này) thì thời gian cache là 1s
- Nếu object chưa 1 trong 2 header nhưng cache time lại lớn hơn Maximum TTL thì ở case này sẽ cache 31536000 giây

Nếu có 1 distribution, tất cả default và không có 1 cái custom behavior nào, và test theo cách như đây:  
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-testing.html 
thì header không có bao gồm cache control hay expires => thời gian expire là default TTL

============================**English**================================
# Meaning of cache parameters:
- The Default TTL will only be valid if the object does not contain a Cache-Control or Expires header. => in this case the cache time will be 86400 seconds (~24 hours). In case there is a header and the caching time set in the header is within the minimum-maximum TTL (1 - 31536000 in this case), the caching time header will overwrite Default, now TTL = TTL is set.
- If the object contains one of the two headers, but the cache time is less than the Minimum TTL (1s in this case), the cache time is 1s.
- If the object contains one of the two headers but the cache time is greater than the Maximum TTL, in this case it will cache 31536000 seconds

If there is a distribution, all default and no custom behavior, and test it like this:
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-testing.html header does not include cache control or expires => expire time is default TTL
