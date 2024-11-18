# P2.-Computer_Vision-Histogram_Equalization
Cải thiện hình ảnh qua cân bằng histogram (Image Enhancement Histogram Equalization)

What is histogram?
Histogram của ảnh kỹ thuật số?
- Biểu đồ tần suất (histogram of a digital image) của một ảnh số với các giá trị màu xám r0, r1, ... , r(L−1) là hàm rời rạc: p(rk) =nk/n
   + rk: số pixels với giá trị xám rk
   + n: tổng số pixels trong ảnh
- Hàm p(rk) biểu thị phần tỷ lệ (fraction) của tổng số pixel có giá trị màu xám rk
- Biểu đồ tần suất (histogram) cung cấp một mô tả toàn cục về diện mạo của hình ảnh (a global description of the appearance of the image).
- Nếu chúng ta xem các giá trị màu xám trong hình ảnh như là các giá trị hiện thực của một biến ngẫu nhiên R, với một mật độ xác suất nào đó, thì biểu đồ tần suất cung cấp một sự xấp xỉ cho mật độ xác suất này. Nói cách khác, Pr (R = rk) ≈ p(rk)

Một vài loại Histogram
- Hình dạng của biểu đồ tần suất cung cấp thông tin hữu ích cho việc tăng cường độ tương phản (contrast enhancement).

1. Histogram Equalization
- Cân bằng biểu đồ tần suất (histogram equalization) là một phương pháp để cải thiện hình ảnh. Phương pháp này là thiết kế một phép biến đổi T(.) sao cho các giá trị màu xám trong ảnh đầu ra được phân bố đều trong khoảng [0, 1].
- Hãy giả sử trong một khoảnh khắc rằng hình ảnh đầu vào cần được tăng cường có các giá trị màu xám liên tục, với r=0 đại diện cho màu đen và r=1 đại diện cho màu trắng.
- Chúng ta cần thiết kế một phép biến đổi giá trị màu xám s=T(r), dựa trên biểu đồ tần suất của hình ảnh đầu vào, để tăng cường hình ảnh.
- Chúng ta giả định rằng:
   + T(r) là một hàm tăng đơn điệu trong khoảng 0≤r≤1 (bảo toàn thứ tự từ màu đen đến màu trắng).
   + T(r) ánh xạ khoảng [0,1] vào [0,1] (bảo toàn khoảng giá trị màu xám cho phép).
- Chúng ta ký hiệu phép biến đổi ngược (inverse transformation) là r=(T ^ (−1)) (s) Chúng ta giả định rằng phép biến đổi ngược cũng thỏa mãn hai điều kiện đã nêu trên.
- Chúng ta coi các giá trị màu xám trong hình ảnh đầu vào và hình ảnh đầu ra như là các biến ngẫu nhiên trong khoảng [0,1].
- Gọi pin(r) và pout(s) lần lượt là mật độ xác suất của các giá trị màu xám trong hình ảnh đầu vào và hình ảnh đầu ra.
- Nếu pin(r) và T(r) đã được biết, và r=(T ^ (−1)) (s) thỏa mãn điều kiện 1, chúng ta có thể viết (kết quả từ lý thuyết xác suất):
pout(s) = [pin(r) * (dr/ds)] r=(T ^ (−1)) (s)
- Một cách để tăng cường hình ảnh là thiết kế một phép biến đổi T(.) sao cho các giá trị màu xám trong ảnh đầu ra được phân bố đồng đều (uniformly distributed) trong khoảng [0,1], tức là pout(s) = 1, 0≤s≤1.
- Về mặt biểu đồ tần suất, hình ảnh đầu ra sẽ có tất cả các giá trị màu xám ở “tỉ lệ bằng nhau” (equal proportion).
- Kỹ thuật này được gọi là cân bằng biểu đồ tần suất (Histogram Equalization)
- Tiếp theo, chúng ta suy diễn rằng các giá trị màu xám trong hình ảnh đầu ra được phân bố đồng đều trong khoảng [0,1].
- Hãy xem xét phép biến đổi: s = T(r) = nguyên hàm (0 -> r) [pin(w)dw , 0 ≤ r ≤ 1
- Lưu ý rằng đây là hàm phân phối tích lũy (cumulative distribution function-CDF) của pin(r) và thỏa mãn hai điều kiện trước đó.
- Từ phương trình trước và sử dụng định lý cơ bản của giải tích, ds/dr = pin(r)
- Therefore, the output histogram is given by: pout (s) = [pin(r) * (1/pin(r))]r=(T ^ (−1)) (s)   = [1]r=(T ^ (−1)) (s) = 1, 0 ≤ s ≤ 1
- Mật độ xác suất đầu ra là đồng đều, bất kể đầu vào là gì.
- Do đó, bằng cách sử dụng một hàm biến đổi bằng với CDF của các giá trị màu xám đầu vào r, chúng ta có thể đạt được một hình ảnh với các giá trị màu xám đồng đều.
- Điều này thường dẫn đến một hình ảnh được cải thiện, với sự gia tăng phạm vi động của các giá trị pixel.

Triển khai: Histogram Equalization

Bước 1: Đối với hình ảnh có các giá trị màu xám rời rạc, tính toán:
pin (rk) = nk/n			0 ≤ rk ≤ 1		0 ≤ k ≤ L − 1
L: Tổng số mức độ màu xám
nk: Số lượng pixel có giá trị màu xám rk
n: Tổng số pixel trong hình ảnh
Bước 2: Dựa trên CDF, tính toán phiên bản rời rạc của phép biến đổi trước đó.
sk = T (rk) = ∑k  j=0 (pin (rj)) 0 ≤ k ≤ L-1

Bàn luận:
Phép cân bằng histogram không phải lúc nào cũng tạo ra kết quả mong muốn, đặc biệt là nếu histogram ban đầu rất hẹp. Nó có thể tạo ra các cạnh và vùng giả. Nó cũng có thể làm tăng độ “hạt-graininess" và “đốm-patchiness" của ảnh.

2. Histogram matching
- Phép cân bằng phổ histogram tạo ra một ảnh có các điểm ảnh (về lý thuyết) phân bố đều (uniformly distributed) trong tất cả các mức xám.
- Đôi khi, điều này có thể không mong muốn. Thay vào đó, chúng ta có thể muốn một phép biến đổi tạo ra ảnh đầu ra với một histogram được xác định trước (pre-specified histogram). Kỹ thuật này được gọi là phép chỉ định histogram (histogram specification).
- Giả sử, ảnh đầu vào có mật độ xác suất là p(r). Chúng ta muốn tìm một phép biến đổi z = H(r), sao cho mật độ xác suất của ảnh mới thu được từ phép biến đổi này là pout(z), mà không nhất thiết phải đồng đều.
- Đầu tiên, áp dụng phép biến đổi.
s = T (r) =ഽ0:r [pin (w)dw], 0 <= r <= 1 (*) Điều này tạo ra một ảnh với mật độ xác suất đồng đều.
- Nếu ảnh đầu ra mong muốn có sẵn, thì phép biến đổi sau đây sẽ tạo ra một ảnh với mật độ đồng đều: v = G (z) = ഽ0:z [pout (w) dw], 0 <= z <= 1 (**)
- Từ các giá trị xám v, chúng ta có thể thu được các giá trị xám z bằng cách sử dụng phép biến đổi ngược, z = G^(−1)(v).
- Nếu thay vì sử dụng các giá trị xám n thu được từ (**), chúng ta sử dụng các giá trị xám s thu được từ (*) ở trên (cả hai đều phân bố đồng đều!), thì phép biến đổi điểm: z=H(r)= G^(-1)[ v=s =T(r)] sẽ tạo ra một ảnh với mật độ được chỉ định p(z), từ một ảnh đầu vào có mật độ p(r)!
- Đối với các mức xám rời, chúng ta có: sk = T (rk) = ∑i=0:j [pin(rj)] 0 ≤ k ≤ L-1
vk = G (zk) = ∑j=0:k [pout(zj)] = sk 0 ≤ k ≤ L-1
- Nếu phép biến đổi zk → G(zk) là một-một, thì phép biến đổi ngược sk → G^(−1)(sk) có thể được xác định dễ dàng, vì chúng ta đang làm việc với một tập hợp nhỏ các giá trị xám rời.
- Trong thực tế, điều này thường không phải là trường hợp (tức là zk → G(zk) không phải là một-một) và chúng ta gán các giá trị xám để phù hợp với histogram đã cho một cách gần nhất có thể.
- Algorithm for histogram matching
Bước 1: Cân bằng hình ảnh đầu vào để có được một hình ảnh với các giá trị xám đồng nhất, sử dụng phương trình rời rạc: 
sk = T (rk) = ∑i=0:j [pin(rj)] 0 ≤ k ≤ L-1
Bước 2: Dựa trên histogram mong muốn để có được một hình ảnh với các giá trị xám đồng nhất, sử dụng phương trình rời rạc.
vk = G (zk) = ∑j=0:k [pout(zj)] = sk 0 ≤ k ≤ L-1
Bước 3: Z = G^(−1) (v = s) → z= G^(−1)[T(r)]




