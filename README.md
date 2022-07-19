```python
# Windows 10 64-bit
# Python 3.9.7 
# 用 Python Tkinter Canvas 寫一個簡單的繪圖Demo
# 參考1 https://www.hashbangcode.com/article/using-events-tkinter-canvas-elements-python
# 參考2 https://shengyu7697.github.io/python-tkinter-canvas/


import tkinter as tk
from tkinter import filedialog
import threading

canvas_width = 500 
canvas_height = 500

mouse_main_button_hold = False

def draw_point(x,y):
    global canvas

    #canvas.create_line(x-5,  y  , x+5, y  , width = 3 ,fill='black')       # 畫線
    canvas.create_oval(x-5, y-5, x+5, y+5,fill="black",outline='black')    # 畫圓or橢圓
    #canvas.create_rectangle(x, y, x+10, y+50, fill="blue",outline="blue")  # 畫矩形
    #canvas.create_polygon( x , y , x+30 , y , x+50 , y+10 , x+20 , y+20)   # 畫多邊形  參數2個一組  x1,y1,x2,y2,x3,y3.......
    #canvas.create_text(x, y, text='哈',font=('Arial', 20) ,fill='black' , anchor='center')  # 畫字    anchor 參數 n, ne, e, se, s, sw, w, nw, or center
    

def MouseFunction(event):
    global mouse_main_button_hold
    global canvas

    if(mouse_main_button_hold == True):
        x = int(event.x)
        y = int(event.y)
        draw_point(x,y)
        #print("x=" + str(x) + "," + "y=" + str(y)) 

    
def MousePress(event):
    global mouse_main_button_hold
    mouse_main_button_hold = True
    x = int(event.x)
    y = int(event.y)
    draw_point(x,y)

    print("press")

def MouseRelease(event):
    global mouse_main_button_hold
    mouse_main_button_hold = False
    print("Release")


def Window_on_Close():    
    global win
    win.destroy()

def Master():
    global win    
    global canvas

    print("<Master ID>:", threading.get_ident())

    win = tk.Tk()
    win.title('Tkinter Canvas Test')
    win.geometry('600x600')
    win.protocol("WM_DELETE_WINDOW", Window_on_Close)
    canvas = tk.Canvas( win , width = canvas_width , height = canvas_height , bg="white" )
    canvas.pack()

    canvas.bind('<Motion>', MouseFunction)         # 滑鼠     "移動" 
    canvas.bind('<ButtonPress-1>', MousePress)     # 滑鼠左鍵  "按下"
    canvas.bind('<ButtonRelease-1>', MouseRelease) # 滑鼠左鍵  "放開"
    win.mainloop()


def BackgroundTask():
    print("<BackgroundTask ID>:", threading.get_ident())


if __name__ == "__main__":
    
    print("<Main ID>:", threading.get_ident())

    task_1 = threading.Thread(target=Master)
    task_2 = threading.Thread(target=BackgroundTask)

    task_2.setDaemon(True)
   
    task_1.start()
    task_2.start()

```

