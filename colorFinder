import tkinter as tk
from tkinter import ttk
from mss import mss
from PIL import Image, ImageTk
from tkinter import colorchooser
import pyperclip
from tkinter import messagebox

LARGE_FONT = ("Cambria", 20)
Norm_Font = ("Verdana", 10)

def messe(root):
    messagebox.showinfo("Copied","The values has been copied to clipboard")
    root.destroy()

def messag(result, root):
    win = tk.Toplevel()
    win.title('Copy')
    rgb, hexa = result
    # rgbb = f"rgb{rgb}"
    rgbb = ''.join([str(round(i)).zfill(3) for i in rgb])
    message = "Which value do you want to copy?"
    tk.Label(win, text=message, font= LARGE_FONT).pack()
    frame = tk.Frame(win)
    frame.pack()
    boldStyle = ttk.Style()
    boldStyle.configure("Bold.TButton", font=('Segoe UI', '10', 'bold'))
    ttk.Button(frame, text='RGB',style="Bold.TButton",command = lambda : copyVal(rgbb,root,win)).pack(side=tk.LEFT,padx=8 ,pady=8)
    ttk.Button(frame, text="Hexa",style="Bold.TButton",command = lambda : copyVal(hexa,root,win)).pack(side=tk.LEFT,padx=8 ,pady=8)

def copyVal(val, root):

    pyperclip.copy(val)
    # win.destroy()
    root.destroy()

def callback(color,root,num):

    result = colorchooser.askcolor(title = "Siris' Color Chooser",color=color)
    RGB, hexa = result
    # rgbb = ''.join([str(round(i)).zfill(3) if i<255 else str(int(i)) for i in RGB ])
    gb = tuple([round(i) for i in RGB])
    rgbb = f"rgb{gb}"
    if RGB is not None and hexa is not None:
        # stt = f"RGB value : {RGB}\nHexadecimal value : {hexa}"
        # messag(result,root)
        if num == 1:
            copyVal(rgbb,root)
        elif num == 2:
            copyVal(hexa,root)

    else:
        root.destroy()

def popupAbout():

    popup = tk.Toplevel()
    popup.resizable(width=False, height=False)
    popup.wm_title("About")

    logo = tk.PhotoImage(file="logo.gif")
    label1= tk.Label(master =popup, image=logo)
    label1.image = logo
    label1.pack(pady=10,side=tk.LEFT)


    label = ttk.Label(popup, text="Color Finder", font=LARGE_FONT)
    label.pack(side="top",fill='x',pady=5)
    frame = tk.Frame(popup)
    frame.pack(side="right")
    labe = ttk.Label(frame, text="Version 0.0.5\n\n", font=Norm_Font,justify="left")
    labe.pack()
    label2 = ttk.Label(frame, text="The creator of this program is Shirish Bajracharya. \nThis program allows user to pick a color from screen \nand automatically copies the RGB and hexadecimal values.\n\nCopyright © 2019 Siris B.",font= ('Comic Sans MS',10))
    label2.pack()
    popup.mainloop()

def popupmsg():
    messagebox.showwarning('Msg', 'Not yet implemented')


def capture_screenshot():
    with mss() as sct:
        monitor = sct.monitors[1]
        sct_img = sct.grab(monitor)
        return Image.frombytes('RGB', sct_img.size, sct_img.bgra, 'raw', 'BGRX')

def showPIL(num):

    root = tk.Toplevel()
    w, h = 1920, 1080
    root.overrideredirect(1)
    root.geometry("1920x1080")
    root.focus_set()

    canvas = tk.Canvas(root,width=1920,height=1080)
    canvas.pack()
    canvas.configure(background='black')
    im = capture_screenshot()
    imgWidth, imgHeight = im.size
    if imgWidth > w or imgHeight > h:
        ratio = min(w/imgWidth, h/imgHeight)
        imgWidth = int(imgWidth*ratio)
        imgHeight = int(imgHeight*ratio)
        im = im.resize((imgWidth,imgHeight), Image.ANTIALIAS)
    image = ImageTk.PhotoImage(im)
    pix = im.load()
    imagesprite = canvas.create_image(w/2,h/2,image=image)
    canvas.config(cursor="@drop.cur")
    root.bind("<Button-1>", lambda e: callback(pix[e.x,e.y],root,num))
    canvas.bind("<Escape>", lambda e: (e.widget.withdraw(), e.widget.quit()))
    root.mainloop()


class TestOOP(tk.Tk):   #inheriting tk.TK

    def __init__(self,*args,**kwargs):

        tk.Tk.__init__(self,*args,**kwargs)
        self.resizable(width=False, height=False)
        self.geometry("250x100")
        tk.Tk.iconbitmap(self,default="color.ico")
        tk.Tk.wm_title(self,"Color Finder By Siris B")



        container = tk.Frame(self)
        container.pack(side="top", fill="both",expand=True)
        container.grid_columnconfigure(0, weight=1)
        container.grid_rowconfigure(0, weight=1)


        menubar = tk.Menu(container)
        filemenu = tk.Menu(menubar)
        filemenu.add_command(label="Save settings", command = popupmsg)
        filemenu.add_separator()
        filemenu.add_command(label='Exit', command=self.destroy)
        menubar.add_cascade(label="File",menu=filemenu)

        filemenu2 = tk.Menu(menubar)
        filemenu2.add_command(label="Help", command = popupmsg)
        filemenu2.add_command(label="Check for Updates...", command = popupmsg)
        filemenu2.add_separator()
        filemenu2.add_command(label='About', command = popupAbout)
        menubar.add_cascade(label="Help",menu=filemenu2)

        tk.Tk.config(self,menu=menubar)

        self.frames = {}

        frame = StartPage(container,self)

        self.frames[StartPage] = frame

        frame.grid(row=0, column=0, sticky="nsew")

        self.show_frame(StartPage)

    def show_frame(self,cont):

        frame = self.frames[cont]
        frame.tkraise()

class StartPage(tk.Frame):

    def __init__(self, parent, controller):

        tk.Frame.__init__(self,parent)
        self.var = tk.IntVar()
        self.var.set(1)

        frame = tk.Frame(self)
        frame.pack()
        self.boldStyle = ttk.Style()
        self.boldStyle.configure("Bold.TButton", font=('Segoe UI', '10', 'bold'))
        button5 = ttk.Button(frame, text="Take Screenshot", command=lambda : showPIL(self.var.get()),
                             style="Bold.TButton")
        button5.pack(side=tk.RIGHT, padx=8, pady=8)
        button5.focus_set()
        button5.bind("<Return>", lambda event: showPIL(self.var.get()))

        tk.Label(self, text="Choose what you want to copy", justify=tk.LEFT, padx=20).pack()
        frame2 = tk.Frame(self)
        frame2.pack()
        tk.Radiobutton(frame2, text="RGB", padx=20, variable=self.var,  value=1).pack(side=tk.LEFT)
        tk.Radiobutton(frame2, text="Hexa", padx=20, variable=self.var, value=2).pack(side=tk.LEFT)



app = TestOOP()
app.mainloop()
