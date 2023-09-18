Để thực hiện truyền dữ liệu trực tiếp giữa hai trình duyệt sử dụng Peer-to-peer data transfer trong JavaScript, chúng ta có thể sử dụng WebRTC DataChannel. Dưới đây là một ví dụ đơn giản:

Trình duyệt A:

```html

<!DOCTYPE html>
<html>
<head>
    <title>Peer-to-peer Data Transfer</title>
</head>
<body>
    <h1>Trình duyệt A</h1>

    <textarea id="messageInput" placeholder="Nhập tin nhắn"></textarea>
    <button onclick="sendMessage()">Gửi</button>

    <script>
        var peerConnection;
        var dataChannel;

        // Tạo peer connection
        peerConnection = new RTCPeerConnection();

        // Tạo data channel trong peer connection
        dataChannel = peerConnection.createDataChannel("dataChannel");

        // Xử lý sự kiện khi có dữ liệu nhận được từ data channel
        dataChannel.onmessage = function(event) {
            console.log("Nhận tin nhắn: " + event.data);
        };

        // Kết nối đến trình duyệt B
        // (Bước này bao gồm việc thiết lập signaling và thiết lập kết nối peer-to-peer)

        // Gửi tin nhắn qua data channel
        function sendMessage() {
            var messageInput = document.getElementById("messageInput");
            var message = messageInput.value;

            // Gửi tin nhắn đến trình duyệt B
            dataChannel.send(message);

            // Xóa nội dung trong ô nhập tin nhắn
            messageInput.value = "";
        }
    </script>
</body>
</html>

```

Trình duyệt B:

```html

<!DOCTYPE html>
<html>
<head>
    <title>Peer-to-peer Data Transfer</title>
</head>
<body>
    <h1>Trình duyệt B</h1>

    <div id="messageOutput"></div>

    <script>
        var peerConnection;
        var dataChannel;

        // Tạo peer connection
        peerConnection = new RTCPeerConnection();

        // Xử lý sự kiện khi có data channel được tạo
        peerConnection.ondatachannel = function(event) {
            dataChannel = event.channel;

            // Xử lý sự kiện khi có dữ liệu nhận được từ data channel
            dataChannel.onmessage = function(event) {
                var messageOutput = document.getElementById("messageOutput");
                messageOutput.innerHTML += "<p>Nhận tin nhắn: " + event.data + "</p>";
            };
        };

        // Kết nối đến trình duyệt A
        // (Bước này bao gồm việc thiết lập signaling và thiết lập kết nối peer-to-peer)
    </script>
</body>
</html>
```
Trong ví dụ trên, trình duyệt A và trình duyệt B sẽ thiết lập kết nối peer-to-peer sử dụng WebRTC và truyền dữ liệu qua DataChannel. Khi trình duyệt A nhập một tin nhắn và nhấn nút "Gửi", tin nhắn sẽ được gửi đến trình duyệt B qua DataChannel. Trình duyệt B sẽ nhận tin nhắn và hiển thị nó trên trang web.

Lưu ý rằng trong thực tế, việc triển khai Peer-to-peer data transfer yêu cầu một hệ thống signaling hoặc server trung gian để truyền tải thông tin giữa các trình duyệt. Ví dụ trên chỉ tập trung vào phần Peer-to-peer data transfer và không bao gồm phần signaling.
