### HoanggHuuChinh
### K215480106007 
# Bài 1
### Hiển thị độ ẩm và nhiệt độ, cho tên chạy trên màn led
![image](https://github.com/user-attachments/assets/897c1bad-83db-4348-a65a-4346efc66de9)
### Hiển thị vị trí 1 ô vuông, vị trí sẽ dựa vào thay đổi của joystick
![image](https://github.com/user-attachments/assets/21316cc0-5d72-41a3-b7dd-a8e6893dd853)
![image](https://github.com/user-attachments/assets/90b8ea1c-cf80-4067-acd9-10601f6c9a0d)
```python
from sense_emu import SenseHat
import time

# Khởi tạo SenseHat
sense = SenseHat()

# Màu sắc cho LED matrix
text_colour = (255, 255, 0)  # Màu vàng
bg_color = (0, 0, 0)         # Nền đen

# Biến joystick
x = y = 4

# Hàm giới hạn giá trị trong phạm vi (0, 7)
def clamp(value, min_value=0, max_value=7):
    return min(max_value, max(min_value, value))

# Hàm di chuyển con trỏ joystick
def move_dot(event):
    global x, y
    if event.action in ('pressed', 'held'):
        x = clamp(x + {
            'left': -1,
            'right': 1,
        }.get(event.direction, 0))
        y = clamp(y + {
            'up': -1,
            'down': 1,
        }.get(event.direction, 0))

# Vòng lặp chính
while True:
    # Hiển thị tên "chinh" trên LED matrix
    sense.show_message("chinh", text_colour=text_colour, back_colour=bg_color, scroll_speed=0.1)
    
    # Đọc nhiệt độ
    temp = sense.temperature
    
    # Đọc độ ẩm
    humidity = sense.humidity
    
    # Xử lý joystick
    for event in sense.stick.get_events():
        move_dot(event)
    
    # Hiển thị trạng thái joystick trên LED matrix
    sense.clear()
    sense.set_pixel(x, y, (0, 255, 0))  # Màu xanh lá cho joystick
    
    # In ra màn hình console
    print(f"Nhiệt độ: {temp:.2f} °C")
    print(f"Độ ẩm: {humidity:.2f} %")
    print(f"Joystick tọa độ: (x={x}, y={y})")
    print("-" * 40)
    
    time.sleep(1)

```
