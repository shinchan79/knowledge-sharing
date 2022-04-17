==================================**Tiếng Việt**=====================================
# ALB báo TargetConnectionErrorCount alarm thì làm gì?
## 1. Hỏi các bên liên quan

- Team infra có động vào không?
- Team app có động vào không?
- AWS có lỗi thời gian đó không

Sau khi chắc chắn 3 thứ trên bình thường thì tiếp tục kiểm tra.

## 2. Các giả thiết

Tham khảo:

CLB/ALB の障害調査依頼を受けた時に確認していること (https://dev.classmethod.jp/articles/elb-trouble-shooting/)

AWS ELB Backend Errors (https://www.bluematador.com/docs/troubleshooting/elb-backend-errors)

Thì thấy có 2 nguyên nhân phổ biến dẫn đến lỗi này:

- The primary cause for connection errors is that the instance is overloaded and rejecting new connections. If the increased connection errors coincides with an increase in the request rate, then this is the likely culprit. 

→ Check xem network in với instance, Check request count tới ALB, Check tiếp xem thời điểm đó có thằng nào fail healthcheck

- Another common cause for connection errors is that traffic is being routed to a port specified in the load balancer’s listener that an instance is not listening to. This can happen if the process listening on the expected port dies unexpectedly, or if a firewall or security group is not allowing access on the port. If the health check port is different than the listener port, it is possible for this to occur even when the health checks succeed. 
  
→ Kiểm tra các configure xem đúng chưa, health check đã setting đúng chưa?

## 3. Bắt đầu kiểm tra

- RequestCount tới ALB tương quan với thời điểm xảy ra lỗi → Lên AWS CloudWatch để graph metrics!
- NetworkIn của các instance web (là target của alb) tương quan với thời điểm xảy ra lỗi → Lên AWS CloudWatch để graph metrics!
- StatusCheckFail của các instance tương quan với thời điểm xảy ra lỗi → Lên AWS CloudWatch để graph metrics!
- Kiểm tra CPUUtilization của các instance web (là target của alb) tương quan với thời điểm xảy ra lỗi→ Lên AWS CloudWatch để graph metrics!
- Kiểm tra xem trong số các target gắn vào có bị stop/start hoặc reboot không → Vào CloudTrail kiểm tra xem có event nào với Event name là RebootInstance(s), StopInstance(s), StartInstance(s) không
- Check log ALB xem có vấn đề gì không (thử grep các HTTP 5xx) trước, trong, và sau thời điểm lỗi (trước và sau khoảng 30 phút - 1 giờ) 
- Kiểm tra lại code define alarm, xem configure ra sao, threshold,.. đã chuẩn chưa → vào thư mục terraform và tìm file define alarm.
- Kiểm tra các configure về health check, firewall,... xem có bất thường gì không? (chú ý xem healthcheck path của app với alb, health check port có đúng chưa?)
- Kiểm tra log app xem có gì bất thường không, log tới static file có bị 500 hay error gì không?

## 4. Phán đoán lỗi

Kiểm tra, đối chiếu những bất thường nếu có tìm được ở bước 3, so sánh với giả thiết bên trên xem sao.

Nếu network in cao, CPU cao thì khả năng cao là do instance quá tải, từ chối connect dẫn đến lỗi này

## 5. Comment:

Quan trọng vẫn là khi xem metrics, phải tìm đc metrics thể hiện sự tương quan

==============================================**English**========================================
# ALB reports TargetConnectionErrorCount alarm, what to do?
## 1. Ask stakeholders:

- Did infra team touch it?
- Did app team touch it?
- Did AWS have errors that time? 

After making sure the above 3 things are normal, continue to check.

## 2. Assumptions

Reference:

CLB/ALB の障害調査依頼を受けた時に確認していること (https://dev.classmethod.jp/articles/elb-trouble-shooting/)

AWS ELB Backend Errors (https://www.bluematador.com/docs/troubleshooting/elb-backend-errors)

There are 2 common causes: 

- The primary cause for connection errors is that the instance is overloaded and rejecting new connections. If the increased connection errors coincides with an increase in the request rate, then this is the likely culprit. 

→ Check the number of network requests to the instance, Check the request count to ALB, was there any instance at that time show healthcheck fail status 

- Another common cause for connection errors is that traffic is being routed to a port specified in the load balancer’s listener that an instance is not listening to. This can happen if the process listening on the expected port dies unexpectedly, or if a firewall or security group is not allowing access on the port. If the health check port is different than the listener port, it is possible for this to occur even when the health checks succeed. 
  
→ Check if health check stetting is correct or not? 

## 3. Start testing

- RequestCount to ALB correlated with time of failure → Go to AWS CloudWatch to graph metrics!
- NetworkIn of web instances (which is the target of the alb) correlates with the time of failure → Go to AWS CloudWatch to graph metrics!
- StatusCheckFail of instances correlates with time of failure → Go to AWS CloudWatch to graph metrics!
- Check CPUUtilization of web instances (which is the target of the alb) in correlation with the time of failure → Go to AWS CloudWatch to graph metrics!
- Check if any of the attached targets are stopped/started or rebooted → Go to CloudTrail to check if there is an event with Event name RebootInstance(s), StopInstance(s), StartInstance(s)
- Check the ALB log for any problems (try grep the 5xx HTTPs) before, during, and after the time of the error (before and after about 30 minutes - 1 hour)
- Check the code defining the alarm metrics to see if correct or not → go to terraform folder and find file define alarm.
- Check the configurations about healthcheck, firewall, ... see if there are any abnormalities? (Take notice to whether the healthcheck path of the app with alb, health check port is correct?)
- Check the app log for anything unusual, log to the static file with 5xx status code or any other errors?

## 4. Error judgment

Check and compare the abnormalities if found in step 3, compare with the hypothesis above.

If the network in is high, the CPU is high, it is most likely because the instance is overloaded, refusing to connect leads to this error

## 5. Comment:

It is still important when looking at metrics, to find metrics that show correlation



