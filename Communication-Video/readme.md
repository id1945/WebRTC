Dưới đây là một ví dụ đơn giản về việc sử dụng WebRTC để truyền thông thời gian thực âm thanh và video giữa hai trình duyệt:

Đầu tiên, chúng ta cần hai trình duyệt hoạt động trên hai thiết bị khác nhau (ví dụ: máy tính và điện thoại di động).

Trên trình duyệt A (máy tính), chúng ta sẽ có mã HTML và JavaScript sau:
```html

<!DOCTYPE html>
<html>
<head>
    <title>WebRTC Audio/Video Communication</title>
</head>
<body>
    <h1>Trình duyệt A</h1>
    <video id="localVideo" autoplay></video>
    <video id="remoteVideo" autoplay></video>

    <script>
        // Lấy stream từ camera và microphone
        navigator.mediaDevices.getUserMedia({ audio: true, video: true })
            .then(function(stream) {
                // Hiển thị stream local trên video element có id là "localVideo"
                var localVideo = document.getElementById('localVideo');
                localVideo.srcObject = stream;

                // Tạo peer connection
                var pc = new RTCPeerConnection();

                // Thêm stream local vào peer connection
                stream.getTracks().forEach(function(track) {
                    pc.addTrack(track, stream);
                });

                // Xử lý sự kiện khi có ice candidate
                pc.onicecandidate = function(event) {
                    if (event.candidate) {
                        // Gửi ice candidate cho trình duyệt B
                        sendIceCandidateToBrowserB(event.candidate);
                    }
                };

                // Xử lý sự kiện khi có remote stream
                pc.ontrack = function(event) {
                    // Hiển thị remote stream lên video element có id là "remoteVideo"
                    var remoteVideo = document.getElementById('remoteVideo');
                    remoteVideo.srcObject = event.streams[0];
                };

                // Tạo offer và gửi cho trình duyệt B
                pc.createOffer()
                    .then(function(offer) {
                        return pc.setLocalDescription(offer);
                    })
                    .then(function() {
                        // Gửi offer cho trình duyệt B
                        sendOfferToBrowserB(pc.localDescription);
                    })
                    .catch(function(error) {
                        console.log(error);
                    });
            })
            .catch(function(error) {
                console.log(error);
            });

        function sendOfferToBrowserB(offer) {
            // Gửi offer cho trình duyệt B (qua mạng hoặc signaling server)
            // ...
        }

        function sendIceCandidateToBrowserB(candidate) {
            // Gửi ice candidate cho trình duyệt B (qua mạng hoặc signaling server)
            // ...
        }
    </script>
</body>
</html>

```
Sau đó, trên trình duyệt B (điện thoại di động), chúng ta sẽ có mã HTML và JavaScript sau:

```html

<!DOCTYPE html>
<html>
<head>
    <title>WebRTC Audio/Video Communication</title>
</head>
<body>
    <h1>Trình duyệt B</h1>
    <video id="localVideo" autoplay></video>
    <video id="remoteVideo" autoplay></video>

    <script>
        // Lấy stream từ camera và microphone
        navigator.mediaDevices.getUserMedia({ audio: true, video: true })
            .then(function(stream) {
                // Hiển thị stream local trên video element có id là "localVideo"
                var localVideo = document.getElementById('localVideo');
                localVideo.srcObject = stream;

                // Tạo peer connection
                var pc = new RTCPeerConnection();

                // Thêm stream local vào peer connection
                stream.getTracks().forEach(function(track) {
                    pc.addTrack(track, stream);
                });

                // Xử lý sự kiện khi có ice candidate
                pc.onicecandidate = function(event) {
                    if (event.candidate) {
                        // Gửi ice candidate cho trình duyệt A
                        sendIceCandidateToBrowserA(event.candidate);
                    }
                };

                // Xử lý sự kiện khi có remote stream
                pc.ontrack = function(event) {
                    // Hiển thị remote stream lên video element có id là "remoteVideo"
                    var remoteVideo = document.getElementById('remoteVideo');
                    remoteVideo.srcObject = event.streams[0];
                };

                function sendAnswerToBrowserA(answer) {
                    // Gửi answer cho trình duyệt A (qua mạng hoặc signaling server)
                    // ...
                }

                function sendIceCandidateToBrowserA(candidate) {
                    // Gửi ice candidate cho trình duyệt A (qua mạng hoặc signaling server)
                    // ...
                }
            })
            .catch(function(error) {
                console.log(error);
            });
    </script>
</body>
</html>
```
Trong ví dụ trên, trình duyệt A và trình duyệt B sẽ lấy stream từ camera và microphone và gửi cho nhau thông qua WebRTC. Mỗi trình duyệt sẽ tạo một peer connection và gửi và nhận các offer, answer và ice candidate thông qua một hệ thống signaling hoặc server trung gian.

Lưu ý rằng trong thực tế, việc triển khai WebRTC yêu cầu một hệ thống signaling hoặc server trung gian để truyền tải thông tin giữa các trình duyệt. Ví dụ trên chỉ tập trung vào phần WebRTC và không bao gồm phần signaling.
