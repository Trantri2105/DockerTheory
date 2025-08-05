# Docker
## Tổng quan
- Docker sử dụng công nghệ ảo hóa cấp hệ điều hành để phân phối phần mềm trong các gói tiêu chuẩn hóa được gọi là container.

- Chức năng cốt lõi của nó là đóng gói một ứng dụng cùng với tất cả các thành phần phụ thuộc của nó—bao gồm thư viện, công cụ hệ thống, mã nguồn và môi trường thực thi (runtime)—vào một đơn vị duy nhất, đáng tin cậy có thể chạy ở bất cứ đâu. Điều này đảm bảo rằng một ứng dụng hoạt động giống hệt nhau trên các môi trường phát triển, kiểm thử và sản xuất.

- Không giống như ảo hóa truyền thống mô phỏng toàn bộ phần cứng, Docker tận dụng các tính năng của nhân (kernel) hệ điều hành chủ. Cụ thể, nó sử dụng các không gian tên (namespaces) để cô lập các tài nguyên như tiến trình, hệ thống tệp và mạng, đồng thời sử dụng các nhóm điều khiển (control groups - cgroups) để quản lý và giới hạn việc sử dụng tài nguyên như CPU và bộ nhớ. Phương pháp ảo hóa cấp hệ điều hành này làm cho các container trở nên cực kỳ nhẹ và khởi động gần như ngay lập tức, vì chúng không cần phải khởi động một hệ điều hành hoàn chỉnh.

- Docker đã trở nên phổ biến vì nó giải quyết một cách hiệu quả các vấn đề cố hữu trong vòng đời phát triển phần mềm.

    - Vấn đề "Nó chạy trên máy của tôi": Đây là vấn đề kinh điển mà Docker giải quyết triệt để. Sự khác biệt giữa các môi trường phát triển, kiểm thử và sản xuất—chẳng hạn như phiên bản hệ điều hành, thư viện, và cấu hình khác nhau—thường dẫn đến việc ứng dụng hoạt động hoàn hảo trên máy của nhà phát triển nhưng lại thất bại khi triển khai. Docker loại bỏ vấn đề này bằng cách đóng gói ứng dụng và môi trường chính xác của nó vào một container, đảm bảo tính nhất quán ở mọi nơi.   

    - Sự hỗn loạn trong quản lý phụ thuộc ("Địa ngục phụ thuộc"): Các ứng dụng hiện đại có một mạng lưới phụ thuộc phức tạp. Các ứng dụng khác nhau trên cùng một máy chủ có thể yêu cầu các phiên bản xung đột của cùng một thư viện. Docker cô lập mỗi ứng dụng và các phụ thuộc của nó trong container riêng, ngăn chặn mọi xung đột và đơn giản hóa việc quản lý.  

    - Hợp lý hóa quy trình phát triển và triển khai: Trước khi có Docker, việc thiết lập môi trường cho một nhà phát triển mới có thể mất nhiều ngày. Quá trình triển khai thường chậm, thủ công và dễ xảy ra lỗi. Docker tiêu chuẩn hóa môi trường dưới dạng một tạo tác (artefact)—image Docker—cho phép các nhà phát triển bắt đầu chỉ với một lệnh docker run và tạo điều kiện cho các đường ống Tích hợp và Phân phối Liên tục (CI/CD) tự động và đáng tin cậy.  

    - Cải thiện hiệu quả sử dụng tài nguyên và khả năng mở rộng: Các phương pháp triển khai truyền thống thường liên quan đến việc mỗi ứng dụng chạy trên một Máy ảo (VM) riêng, điều này rất tốn tài nguyên. Các container nhẹ của Docker cho phép mật độ cao hơn nhiều, chạy nhiều ứng dụng hơn trên cùng một phần cứng, do đó giảm chi phí. Bản chất nhẹ này cũng giúp việc mở rộng ứng dụng bằng cách khởi chạy các container mới trở nên cực kỳ nhanh chóng.

- Sự khác biệt giữa container và máy ảo (VM):
    - Máy ảo (Virtual Machines - VMs): VMs ảo hóa phần cứng vật lý. Một trình ảo hóa (hypervisor) chạy trên hệ điều hành chủ (hoặc trực tiếp trên phần cứng "trần") và tạo ra nhiều ngăn xếp phần cứng ảo. Mỗi VM sau đó chạy một hệ điều hành "khách" hoàn chỉnh, bao gồm nhân, thư viện và ứng dụng riêng. Điều này dẫn đến các gói phần mềm nặng, thường có kích thước hàng chục gigabyte (GB).
    - Container Docker: Container ảo hóa hệ điều hành. Docker Engine chạy trên hệ điều hành chủ và các container chia sẻ nhân của máy chủ đó. Mỗi container chỉ đóng gói ứng dụng và các phụ thuộc cụ thể của nó (thư viện, tệp nhị phân), chạy như một tiến trình cô lập trong không gian người dùng. Sự khác biệt về kiến trúc này là chìa khóa cho bản chất nhẹ của chúng, với các image thường chỉ có kích thước hàng chục megabyte (MB).
    - Hiệu suất và Thời gian khởi động: Vì container không cần khởi động một hệ điều hành đầy đủ, chúng khởi động gần như ngay lập tức, tương tự như khởi động một tiến trình thông thường. Ngược lại, VMs có thể mất vài phút để khởi động hệ điều hành khách của chúng.
    - Sử dụng tài nguyên và Mật độ: Container có chi phí hoạt động (overhead) thấp hơn đáng kể. Chúng chia sẻ nhân của máy chủ và không yêu cầu một hệ điều hành đầy đủ cho mỗi phiên bản. Điều này cho phép mật độ cao hơn nhiều—chạy nhiều container hơn trên một máy chủ so với VMs, dẫn đến việc sử dụng tài nguyên tốt hơn và tiết kiệm chi phí. 
    - Tính di động: Lời hứa của Docker là "xây dựng một lần, chạy ở mọi nơi". Một image container Docker gói gọn mọi thứ mà ứng dụng cần và có thể chạy trên bất kỳ máy chủ nào có Docker Engine (Linux, Windows, macOS, đám mây), đảm bảo một môi trường nhất quán. VMs ít di động hơn giữa các trình ảo hóa và nền tảng đám mây khác nhau mà không có các vấn đề tương thích tiềm ẩn. 

## Docker Engine
- Kiến trúc cốt lõi của Docker tuân theo mô hình client-server, bao gồm:
    - Docker Daemon (dockerd): Đây là tiến trình máy chủ chạy liên tục, thực hiện các công việc nặng nhọc. Nó lắng nghe các yêu cầu từ API và quản lý các đối tượng Docker như image, container, mạng và volume. Nó được coi là "bộ não" của Docker.
    - Docker Client (docker): Đây là giao diện dòng lệnh (CLI) mà người dùng tương tác. Khi người dùng gõ các lệnh như docker run hoặc docker build, client sẽ dịch các lệnh này thành các yêu cầu API và gửi chúng đến daemon. Client có thể giao tiếp với một daemon trên cùng một máy hoặc một daemon từ xa, mang lại sự linh hoạt trong quản lý.

- Các thành phân chính trong Docker:
    - Images: Đây là các mẫu chỉ đọc chứa các hướng dẫn để tạo một container Docker. Chúng có thể được coi là "bản thiết kế" cho một ứng dụng và môi trường của nó, bao gồm mã nguồn, môi trường thực thi, thư viện và các tệp cấu hình. Các image Docker được cấu thành từ một loạt các lớp (layers) chỉ đọc được xếp chồng lên nhau. Mỗi chỉ thị trong một Dockerfile (ví dụ: RUN, COPY) sẽ tạo ra một lớp mới. Kiến trúc phân lớp này cực kỳ hiệu quả. Khi bạn thay đổi một image, chỉ có lớp bị sửa đổi và các lớp tiếp theo mới được xây dựng lại. Các lớp được chia sẻ giữa các image, giúp tiết kiệm dung lượng đĩa và tăng tốc độ tải.
    - Containers: Đây là các phiên bản có thể chạy được của một image. Chúng là các môi trường nhẹ, cô lập nơi các ứng dụng thực sự chạy. Một container có thể được khởi động, dừng, di chuyển và xóa thông qua CLI hoặc API. 
    - Volumes: Đây là cơ chế được ưu tiên để lưu trữ dữ liệu lâu dài do các container tạo ra và sử dụng. Volumes được quản lý bởi Docker và tồn tại bên ngoài vòng đời của container, cho phép dữ liệu tồn tại ngay cả khi container bị xóa.
    - Networks: Networks cung cấp các kênh giao tiếp giữa các container. Docker hỗ trợ nhiều trình điều khiển mạng khác nhau (như bridge, overlay) để xử lý các kịch bản giao tiếp khác nhau, từ một máy chủ duy nhất đến các cụm đa máy chủ.
    - Registries là một hệ thống lưu trữ và phân phối cho các image.

## Các câu lệnh trong Docker
- Các lệnh Container thiết yếu
    - `docker run`: Lệnh nền tảng để tạo và khởi động một container mới từ một image. Các cờ quan trọng bao gồm `-d` để chạy ở chế độ nền (detached), `-p` để ánh xạ cổng (ví dụ: `-p 8080:80`), `--name` để đặt tên cho container, và `-v` để gắn volume. 
    - `docker ps`: Liệt kê các container đang chạy. Cờ -a sẽ hiển thị tất cả các container, bao gồm cả những container đã dừng.
    - `docker stop`, `docker start`, `docker restart`: Các lệnh quản lý vòng đời cơ bản để dừng, khởi động và khởi động lại các `container`.
    - `docker logs`: Lấy logs của một container. Các cờ hữu ích bao gồm `-f` để theo dõi đầu ra nhật ký trực tiếp và `--tail` để chỉ hiển thị một số dòng cuối cùng.
    - `docker exec`: Thực thi một lệnh bên trong một container đang chạy.
    - `docker inspect`: Hiển thị thông tin chi tiết về một container ở định dạng `JSON`.

- Các câu lệnh Images thiết yếu:
    - `docker build`: Xây dựng một image từ một Dockerfile và build context.
    - `docker images` (hoặc `docker image ls`): Liệt kê tất cả các image có sẵn trên máy.
    - `docker pull`: Tải một image từ một registry (mặc định là Docker Hub).
    - `docker push`: Tải một image lên một registry.

## Lưu trữ dữ liệu trong Docker
- Dữ liệu bên trong lớp có thể ghi của container là tạm thời và sẽ bị mất khi container bị xóa. Do đó, việc sử dụng các giải pháp lưu trữ bên ngoài là cần thiết cho các ứng dụng cần lưu trữ trạng thái. Docker cung cấp hai cơ chế chính cho việc này: volumes và bind mounts.
- Volumes
    - Khái niệm: Volumes là các kho lưu trữ do Docker quản lý, được tạo ra trong một khu vực lưu trữ riêng của Docker trên máy chủ và được cô lập khỏi cấu trúc tệp của máy chủ.   
    - Ưu điểm: Đây là phương pháp được khuyến nghị cho các ứng dụng sản xuất và có trạng thái. Volumes dễ dàng sao lưu và di chuyển, hoạt động trên cả Linux và Windows, có thể được chia sẻ an toàn giữa nhiều container, và có thể được quản lý thông qua Docker CLI (ví dụ: docker volume create, docker volume ls).  
    - Nhược điểm: Không dễ dàng truy cập hoặc sửa đổi trực tiếp từ máy chủ, vì chúng được quản lý hoàn toàn bởi Docker. 

- Bind Mounts
    - Khái niệm: Bind mounts gắn một tệp hoặc thư mục trực tiếp từ hệ thống tệp của máy chủ vào container. Người dùng chỉ định đường dẫn chính xác trên máy chủ.   
    - Ưu điểm: Rất phù hợp cho các quy trình phát triển. Nó cho phép các nhà phát triển chỉnh sửa mã nguồn trên máy chủ bằng IDE yêu thích của họ và thấy các thay đổi được phản ánh ngay lập tức bên trong container mà không cần xây dựng lại image.  
    - Nhược điểm: Nó liên kết chặt chẽ container với cấu trúc hệ thống tệp của máy chủ, làm cho nó không di động. Nó cũng có thể gây ra các vấn đề bảo mật vì một container có thể sửa đổi hệ thống tệp của máy chủ, bao gồm cả các tệp hệ thống quan trọng.

## Docker networks
- Các loại network trong Docker
    - bridge (Mặc định): Nó tạo ra một mạng nội bộ riêng trên máy chủ. Các container được kết nối vào cùng một mạng bridge do người dùng định nghĩa có thể giao tiếp với nhau bằng tên container, vì Docker cung cấp cơ chế phân giải DNS tự động. Đây là lựa chọn tiêu chuẩn cho các ứng dụng chạy trên một máy chủ duy nhất. Để truy cập các dịch vụ từ bên ngoài, các cổng phải được công bố (published) bằng cờ -p.
    - host: Trình điều khiển này loại bỏ sự cô lập mạng. Container chia sẻ trực tiếp mạng của máy chủ. Điều này mang lại hiệu suất tối đa vì không có lớp NAT nào, nhưng nó hy sinh sự cô lập và bảo mật. Nó hữu ích cho các ứng dụng yêu cầu hiệu suất cao và không cần sự cô lập mạng, hoặc cần tương tác với các dịch vụ mạng của máy chủ.
    - overlay: Trình điều khiển này được thiết kế để kết nối nhiều Docker daemon với nhau, cho phép mạng đa máy chủ. Đây là thành phần thiết yếu cho việc điều phối container với Docker Swarm, cho phép các container trên các máy chủ khác nhau giao tiếp liền mạch như thể chúng ở trên cùng một mạng.
    - none: Vô hiệu hóa hoàn toàn mạng cho một container.

## Dockerfile
- Một Dockerfile là một tài liệu văn bản chứa một loạt các chỉ thị mà lệnh docker build sử dụng để tự động lắp ráp một image.
    - `FROM`: Chỉ định image cơ sở mà từ đó image mới sẽ được xây dựng. Đây phải là chỉ thị đầu tiên trong một Dockerfile. 
    - `RUN`: Thực thi các lệnh trong một lớp mới. Được sử dụng để cài đặt các gói phần mềm, cập nhật hệ thống, v.v..
    - `COPY`: Sao chép các tệp hoặc thư mục từ bối cảnh xây dựng (thư mục cục bộ) vào hệ thống tệp của image.
    - `ADD`: Tương tự như COPY nhưng có thêm các tính năng như tìm nạp URL và tự động giải nén các tệp lưu trữ tar.
    - `WORKDIR`: Thiết lập thư mục làm việc cho các chỉ thị tiếp theo như RUN, CMD, ENTRYPOINT, COPY, và ADD.  
    - `CMD`: Cung cấp lệnh mặc định và/hoặc các tham số cho một container đang thực thi. Lệnh này có thể bị ghi đè tại thời điểm chạy.  
    - `ENTRYPOINT`: Cấu hình một container để chạy như một tệp thực thi. Các đối số từ docker run sẽ được nối vào sau ENTRYPOINT.  
    - `EXPOSE`: Ghi lại các cổng mạng mà container lắng nghe tại thời điểm chạy. Nó không thực sự công bố cổng; điều đó được thực hiện bằng cờ -p trong docker run.  
    - `ENV`: Thiết lập các biến môi trường.  
    - `USER`: Thiết lập tên người dùng (hoặc UID) để chạy các lệnh tiếp theo và container cuối cùng.
- Multi-Stage Builds:
    - Xây dựng đa giai đoạn sử dụng nhiều chỉ thị `FROM` trong một Dockerfile duy nhất để tách biệt môi trường xây dựng khỏi môi trường chạy cuối cùng.
    - Đây là cách hiệu quả nhất để tạo ra các image gọn nhẹ. Image cuối cùng chỉ chứa  ứng dụng đã được biên dịch và các phụ thuộc của nó, loại bỏ tất cả các công cụ xây dựng (như trình biên dịch, SDK và các phụ thuộc phát triển). Điều này làm giảm đáng kể kích thước image.

## Docker Compose
- Docker Compose là một công cụ thiết yếu trong hệ sinh thái Docker, được thiết kế để đơn giản hóa việc định nghĩa và chạy các ứng dụng đa container.
- Trong khi Docker xử lý các container riêng lẻ, các ứng dụng hiện đại thường bao gồm nhiều dịch vụ phụ thuộc lẫn nhau—chẳng hạn như một máy chủ web, một cơ sở dữ liệu và một dịch vụ bộ đệm. Việc quản lý các dịch vụ này một cách riêng lẻ bằng các lệnh docker run dài dòng và liên kết mạng thủ công có thể trở nên phức tạp và dễ xảy ra lỗi. Docker Compose giải quyết vấn đề này bằng cách cho phép bạn định nghĩa toàn bộ ngăn xếp ứng dụng của mình trong một tệp YAML duy nhất, dễ đọc. Với một lệnh duy nhất, docker compose up, bạn có thể khởi tạo và chạy tất cả các dịch vụ, mạng và volume cần thiết, đảm bảo một quy trình làm việc nhất quán và có thể tái tạo trên các môi trường phát triển, kiểm thử và sản xuất.
- Tệp compose.yaml (hoặc docker-compose.yml) là trung tâm của Docker Compose. Nó sử dụng cú pháp YAML để định nghĩa các thành phần của ứng dụng.
    - services: Phần quan trọng nhất, nơi mỗi container được định nghĩa là một "dịch vụ" riêng biệt (ví dụ: web, database, cache).  
    - networks: Cho phép bạn định nghĩa các mạng tùy chỉnh để các dịch vụ kết nối, cung cấp sự cô lập và tổ chức tốt hơn so với mạng mặc định.
    - volumes: Định nghĩa các volume có tên để lưu trữ dữ liệu lâu dài. Các volume này có thể được chia sẻ giữa các dịch vụ và vòng đời của chúng độc lập với các container.
    - secrets: Cung cấp một cách an toàn để quản lý dữ liệu nhạy cảm. Secrets chỉ hoạt động ở chế độ Swarm và được gắn vào container dưới dạng các tệp trong bộ nhớ.
- Bên trong phần services, mỗi dịch vụ được định nghĩa với một tập hợp các thuộc tính cấu hình:
    - image: Chỉ định image sẽ sử dụng cho dịch vụ, được kéo từ một registry như Docker Hub.
    - build: Chỉ định đường dẫn đến một thư mục chứa Dockerfile. Compose sẽ xây dựng một image tùy chỉnh cho dịch vụ.
    - ports: Ánh xạ các cổng từ máy chủ đến container theo định dạng "HOST:CONTAINER". 
    - environment: Thiết lập các biến môi trường bên trong container. Có thể được cung cấp dưới dạng danh sách hoặc map.
    - env_file: Tải các biến môi trường từ một tệp. Hữu ích để tách biệt cấu hình khỏi tệp Compose.
    - volumes: Gắn các đường dẫn máy chủ (bind mounts) hoặc các volume có tên vào container.
    - networks: Kết nối dịch vụ với các mạng được chỉ định.
    - depends_on: Kiểm soát thứ tự khởi động của các dịch vụ, đảm bảo các phụ thuộc được khởi động trước.
    - restart: Định nghĩa chính sách khởi động lại cho container (no, always, on-failure, unless-stopped).
- Các câu lệnh để quản lý Docker Compose:
    - docker compose up: Xây dựng (nếu cần), tạo, khởi động và đính kèm vào các container cho một ứng dụng. Thêm cờ -d để chạy ở chế độ nền (detached).
    - docker compose down	Dừng và xóa các container, mạng. Thêm cờ --volumes để xóa cả các volume có tên.
    - docker compose ps: Liệt kê các container đang chạy được quản lý bởi Compose.
    - docker compose logs: Hiển thị đầu ra nhật ký từ các dịch vụ. Sử dụng -f để theo dõi nhật ký trực tiếp.
    - docker compose build: Xây dựng hoặc xây dựng lại các image cho các dịch vụ.
    - docker compose pull: Kéo các image dịch vụ từ registry.
    - docker compose start: Khởi động các dịch vụ hiện có.
    - docker compose stop: Dừng các dịch vụ hiện có.
    - docker compose restart: Khởi động lại các dịch vụ.
    - docker compose exec <service> <command>: Thực thi một lệnh bên trong một container dịch vụ đang chạy.

## Docker Swarm
- Docker Swarm, hay chính xác hơn là "chế độ Swarm", là một công cụ điều phối container được tích hợp sẵn trong Docker Engine. Nó cho phép bạn tạo và quản lý một cụm (cluster) gồm nhiều máy chủ Docker, biến chúng thành một hệ thống ảo duy nhất để triển khai và mở rộng quy mô ứng dụng một cách liền mạch.
- Các tính năng chính của Docker Swarm:
    - Quản lý cụm tích hợp: Tạo và quản lý một cụm các Docker Engine trực tiếp bằng Docker CLI mà không cần phần mềm điều phối bổ sung.   
    - Mô hình dịch vụ khai báo: Bạn định nghĩa trạng thái mong muốn của ứng dụng (ví dụ: "chạy 3 bản sao của dịch vụ web") và Swarm sẽ tự động duy trì trạng thái đó.  
    - Tự động cân bằng tải: Swarm tích hợp sẵn cơ chế cân bằng tải, phân phối các yêu cầu đến các container trong một dịch vụ một cách đồng đều.  
    - Khám phá dịch vụ (Service Discovery): Các container trong cùng một mạng Swarm có thể tìm thấy và giao tiếp với nhau bằng tên dịch vụ, nhờ vào một máy chủ DNS được tích hợp sẵn.  
    - Khả năng mở rộng (Scaling): Dễ dàng tăng hoặc giảm số lượng container cho một dịch vụ bằng một lệnh duy nhất, và Swarm sẽ tự động điều chỉnh.  
    - Tự phục hồi (Desired State Reconciliation): Nếu một node bị lỗi và các container trên đó ngừng hoạt động, Swarm manager sẽ tự động tạo các bản sao mới trên các node khỏe mạnh khác để duy trì trạng thái mong muốn.  
    - Cập nhật cuốn chiếu (Rolling Updates): Áp dụng các bản cập nhật cho dịch vụ một cách tuần tự mà không gây gián đoạn, và có thể dễ dàng quay lui (rollback) nếu có sự cố.  
    - Bảo mật mặc định: Giao tiếp giữa các node trong swarm được mã hóa và xác thực lẫn nhau bằng TLS theo mặc định.
- Kiến trúc Docker Swarm:
    - Một swarm là một cụm gồm nhiều Docker Engine hoạt động cùng nhau. Các máy chủ này, có thể là máy vật lý hoặc máy ảo, được gọi là các node. Kiến trúc của Swarm được xây dựng dựa trên mô hình manager-worker đơn giản nhưng mạnh mẽ. 
    - Một cụm Swarm bao gồm hai loại node với các vai trò khác nhau :  
        - Node Manager: Đây là bộ não của cụm. Các node manager chịu trách nhiệm cho các công việc quản lý, bao gồm duy trì trạng thái cụm, lưu trữ và quản lý cấu hình của toàn bộ swarm, bao gồm các dịch vụ, mạng và secret, lập lịch (scheduling) quyết định xem các task (container) sẽ được chạy trên node worker nào, phục vụ API cung cấp các điểm cuối API HTTP của chế độ Swarm để các công cụ bên ngoài và Docker CLI có thể tương tác với cụm.  

        - Node Worker: Vai trò duy nhất của các node worker là nhận và thực thi các task (tức là chạy các container) được giao bởi node manager. Chúng không tham gia vào việc ra quyết định quản lý cụm. Mỗi worker chạy một agent để báo cáo trạng thái của các task về cho manager.
    - Để đảm bảo khả năng chịu lỗi, một cụm Swarm production nên có nhiều node manager. Các manager này sử dụng một triển khai của thuật toán đồng thuận Raft để duy trì một trạng thái nội bộ nhất quán của toàn bộ swarm.
        - Bầu chọn Leader: Trong số các manager, một leader được bầu ra để đưa ra tất cả các quyết định điều phối và quản lý. Nếu leader gặp sự cố, các manager còn lại sẽ tự động bầu ra một leader mới.   
        - Quorum (Số đại biểu): Để cụm hoạt động ổn định, cần phải duy trì một quorum, tức là đa số các manager phải hoạt động. Một cụm gồm N manager có thể chịu được sự cố của tối đa (N-1)/2 manager.