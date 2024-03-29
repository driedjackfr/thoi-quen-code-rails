# Thói quen code Rails

*Xây dựng thói quen lập trình Ruby on Rails từ những điều nhỏ nhặt hằng ngày.*

Với việc code hằng ngày, ta không muốn phải đắn đo suy nghĩ hoặc lựa chọn những điều mà lâu dần ta xem nó như một thói quen hay phong cách tốt cho ta và những dự án ta làm. Thế nên tôi lập ra danh sách những điều sẽ được lặp lại thường xuyên, được áp dụng hầu hết cho mọi dự án, những kiểu viết code, cách đặt hay dùng code,... và gọi chúng là thói quen tốt khi code. Tất cả đều được tìm hiểu, nghiên cứu và so sánh. Tất nhiên, mọi thứ chỉ là tương đối, nó có thể tốt trong trường hợp này nhưng ngược lại với trường hợp khác, nên vì vậy điều quan trong vẫn là tuỳ trường hợp mà dùng, tuỳ cơ mà hành sự.

Mục tiêu cuối cùng cốt giúp cho việc lập trình Ruby on Rails nhẹ nhàng và vui vẻ hơn.

---

## Nên comment dòng `frozen_string_literal: true` đầu mỗi file ruby

- Để đóng băng tất cả chuỗi của file đó và một phần tăng performance cho ứng dụng.

- Ví dụ: trong file `application_controller.rb`

```ruby
# frozen_string_literal: true

class ApplicationController < ActionController::Base
end
```

Tại sao lại đóng băng chuỗi gốc (string literal)? Với mỗi đối tượng chuỗi được tạo từ cách khai báo như này `'x'` đều chiếm một vùng trong bộ nhớ, nếu gọi thêm một `'x'` khác thì dù thể hiện hai nội dung giống nhau nhưng đó là hai đối tượng khác nhau chiếm hai vùng nhớ khác nhau, dẫn tới việc ta chỉ muốn có một đối tượng cho một chuỗi cụ thể. Hằng giải quyết điều đó, chỉ cần đặt biến hằng rồi khi cần dùng chuỗi đó thì gọi tới hằng tương ứng, nhưng có một vấn đề là hằng số trong ruby có thể thay đổi được, cho nên ta cần phải đóng băng nó bằng dòng comment `# frozen_string_literal: true` ảnh hưởng toàn file. Từ đó cũng góp phần làm tăng hiệu năng cho ứng dụng.

Tham khảo ở:
- [What does the comment “frozen_string_literal: true” do?](https://stackoverflow.com/questions/37799296/what-does-the-comment-frozen-string-literal-true-do)
- [Ruby note](https://bugs.ruby-lang.org/issues/8976#note-30)
- [An Introduction to Frozen String Literals - Freelancing Gods](https://freelancing-gods.com/2017/07/27/an-introduction-to-frozen-string-literals.html)

## Cách dòng để phân biệt vùng liên quan

Ta có thể phân vùng code cho mắt dễ đọc, dễ định hình hơn bằng cách dòng. Nhóm những dòng code có cùng chức năng, tách riêng từng đoạn code nếu mỗi đoạn là độc lập ví dụ như khai báo lớp, khai báo hàm,...

Ví dụ:

```ruby
# frozen_string_literal: true

require 'trò_chơi'
require 'chiến_thuật'

class TaiKhoan < ApplicationRecord
  include ThoiGian
  include DauOc
  
  SO_TRAN_BI_BAN = 5
  TEN_GAME = 'Dota'
  
  belongs_to :người_chơi
  belongs_to :quốc_gia
  
  has_many :trận
  has_many :đồ
  
  validates :tên, presence: true, uniqueness: true
  validates :tiền, numericality: true
  
  before_validate :bỏ_khoảng_trắng_trong_tên
  
  before_save :tăng_số_lần_chơi
  
  scope :vip, -> { where(loại: :vip) }
  scope :ban, -> { where('số_lần_huỷ_trận > ?', SO_TRAN_BI_BAN }
  
  class << self
    def tài_khoản_nam
      # code linh tinh
    end
    
    def tài_khoản_nữ
      # code linh tinh
    end
  end
  
  def chơi_hay_không?
    # code linh tinh
  end
  
  private
  
  def tên_kèm_quốc_gia
    # code linh tinh
  end
end
```
