# timer







    from PyQt6.QtWidgets import QApplication, QMainWindow, QVBoxLayout, QWidget, QPushButton, QLabel
    from PyQt6.QtCore import QTimer, Qt
    from PyQt6.QtGui import QPainter, QColor, QPen, QFont
    import sys

    class CircularTimer(QMainWindow):
        def __init__(self):
            super().__init__()
            self.setWindowTitle("Круговой таймер")
            self.setFixedSize(400, 400)
        
            self.time_left = 10  
            self.is_running = False
        
        
            self.central_widget = QWidget()
            self.setCentralWidget(self.central_widget)
            self.layout = QVBoxLayout()
            self.central_widget.setLayout(self.layout)
        
      
            self.label = QLabel(str(self.time_left))
            self.label.setAlignment(Qt.AlignmentFlag.AlignCenter)
            self.label.setFont(QFont("Arial", 48))
            self.layout.addWidget(self.label)
        
       
            self.start_button = QPushButton("Старт")
            self.start_button.clicked.connect(self.start_timer)
            self.layout.addWidget(self.start_button)
        
            self.pause_button = QPushButton("Пауза")
            self.pause_button.clicked.connect(self.pause_timer)
            self.layout.addWidget(self.pause_button)
        
            self.reset_button = QPushButton("Сброс")
            self.reset_button.clicked.connect(self.reset_timer)
            self.layout.addWidget(self.reset_button)
        
      
            self.timer = QTimer()
            self.timer.timeout.connect(self.update_timer)
    
        def start_timer(self):
            if not self.is_running:
                self.is_running = True
                self.timer.start(1000) 
    
        def pause_timer(self):
            if self.is_running:
                self.is_running = False
                self.timer.stop()
    
        def reset_timer(self):
            self.is_running = False
            self.timer.stop()
            self.time_left = 10
            self.label.setText(str(self.time_left))
            self.update()  
    
        def update_timer(self):
            if self.time_left > 0:
                self.time_left -= 1
                self.label.setText(str(self.time_left))
                self.update()  
            else:
                self.timer.stop()
                self.is_running = False
    
        def paintEvent(self, event):
            painter = QPainter(self)
            painter.setRenderHint(QPainter.RenderHint.Antialiasing)
        
     
            pen = QPen(QColor("lightgray"), 8)
            painter.setPen(pen)
            painter.drawArc(50, 50, 300, 300, 0, 360 * 16)
        
       
            progress = (self.time_left / 10) * 360  
            pen = QPen(QColor("#4285F4"), 8)
            painter.setPen(pen)
            painter.drawArc(50, 50, 300, 300, 90 * 16, -progress * 16)

    if __name__ == "__main__":
        app = QApplication(sys.argv)
        window = CircularTimer()
        window.show()
        sys.exit(app.exec())
