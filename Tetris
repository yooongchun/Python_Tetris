#!/usr/bin/python3
# -*- coding:utf-8 -*-

import sys, random
from PyQt5.QtWidgets import QApplication, QDesktopWidget, QHBoxLayout, QVBoxLayout, QLabel, \
    QPushButton, QFrame, QLCDNumber, QSlider

from PyQt5.QtGui import QIcon, QPainter, QPen, QBrush, QColor, QFont
from PyQt5.QtCore import Qt, QBasicTimer


# 形状
class Shape:
    def __init__(self):
        # 19种形状：tuple参数分别代表：(形状代号，左、右、上、下边缘距离,类型)
        self.num = 19
        self.type1 = 1
        self.type2 = 2
        self.type3 = 3
        self.type4 = 4
        self.type5 = 5
        self.vL = (1, 0, 0, 0, 3, self.type1)
        self.hL = (2, 0, 3, 0, 0, self.type1)
        self.S = (3, 0, 1, 0, 1, self.type2)
        self.lZ = (4, 0, 2, 0, 1, self.type3)
        self.ruZ = (5, 0, 1, 1, 1, self.type3)
        self.rZ = (6, 0, 2, 1, 0, self.type3)
        self.luZ = (7, 0, 1, 0, 2, self.type3)
        self.lvuF = (8, 0, 1, 0, 2, self.type4)
        self.rvuF = (9, 0, 1, 0, 2, self.type4)
        self.lhdF = (10, 0, 2, 0, 1, self.type4)
        self.rhdF = (11, 0, 2, 0, 1, self.type4)
        self.rvdF = (12, 0, 1, 0, 2, self.type4)
        self.lvdF = (13, 0, 1, 2, 0, self.type4)
        self.rhuF = (14, 0, 2, 1, 0, self.type4)
        self.lhuF = (15, 0, 2, 0, 1, self.type4)
        self.uW = (16, 0, 2, 1, 0, self.type5)
        self.dW = (17, 0, 2, 0, 1, self.type5)
        self.lW = (18, 0, 1, 1, 1, self.type5)
        self.rW = (19, 0, 1, 0, 2, self.type5)
        self.name = (
            (1, 0, 0, 0, 3, self.type1), (2, 0, 3, 0, 0, self.type1),
            (3, 0, 1, 0, 1, self.type2), (4, 0, 2, 0, 1, self.type3),
            (6, 0, 2, 1, 0, self.type3), (7, 0, 1, 0, 2, self.type3),
            (5, 0, 1, 1, 1, self.type3), (8, 0, 1, 0, 2, self.type4),
            (13, 0, 1, 2, 0, self.type4), (9, 0, 1, 0, 2, self.type4),
            (12, 0, 1, 0, 2, self.type4),
            (15, 0, 2, 0, 1, self.type4), (10, 0, 2, 0, 1, self.type4),
            (14, 0, 2, 1, 0, self.type4), (11, 0, 2, 0, 1, self.type4),
            (16, 0, 2, 1, 0, self.type5), (17, 0, 2, 0, 1, self.type5),
            (18, 0, 1, 1, 1, self.type5),
            (19, 0, 1, 0, 2, self.type5))

        self.color = (QColor(250, 150, 50), QColor(100, 100, 100), QColor(100, 150, 150), QColor(150, 100, 100))
        self.num_col = len(self.color)


# Game
class Game:
    def __init__(self):
        self.__board = Board()


# 界面
class Board(QFrame):
    def __init__(self):
        super().__init__()
        self.__num_y = 23
        self.__num_x = 25
        self.__time_step = 400

        self.__initPara()
        self.__initUI()
        self.__initNet()
        self.setFocusPolicy(Qt.StrongFocus)

    # 初始化参数
    def __initPara(self):
        self.__score = 0
        self.__level = 0
        self.__timer = QBasicTimer()
        self.__FACTOR = 4 / 5
        self.__FACTOR_SCREEN = 0.6
        self.__canvas_w = self.geometry().width() * self.__FACTOR
        self.__canvas_h = self.geometry().height()
        self.__szy = int(self.__canvas_h / self.__num_y)
        self.__szx = int(self.__canvas_w / self.__num_x)
        self.__gameOverFlag = False
        self.__net = []

        self.__mshape = Shape()
        self.__block = Block(1, 1, self.__mshape.name[random.randint(0, self.__mshape.num - 1)], self.__mshape,
                             self.__mshape.color[random.randint(0, self.__mshape.num_col - 1)])

    # 初始化网格列表
    def __initNet(self):
        self.__net = [[0 for j in range(self.__num_x - 1)] for j in range(self.__num_y - 1)]

    # 初始化界面
    def __initUI(self):
        hb1 = QHBoxLayout()
        score_info_la = QLabel('Score: ')
        self.__score_la = QLabel('0')
        hb1.addWidget(score_info_la)
        hb1.addWidget(self.__score_la)
        hb1.addStretch(1)

        hb2 = QHBoxLayout()
        level_info_la = QLabel('Level: ')
        self.__level_la = QLabel('0')
        hb2.addWidget(level_info_la)
        hb2.addWidget(self.__level_la)
        hb2.addStretch(1)

        self.__speed_la = QLabel()
        self.__speed_la.setText(str((1010 - self.__time_step) / 10))
        self.__speed_label = QLabel('Speed:')
        self.__sd_slider = QSlider()
        self.__sd_slider.setOrientation(Qt.Horizontal)
        self.__sd_slider.setMaximum(1)
        self.__sd_slider.setMaximum(100)
        self.__sd_slider.setValue(int((1010 - self.__time_step) / 10))
        self.__sd_slider.valueChanged.connect(self.__LineEdt)

        hb3 = QHBoxLayout()
        hb3.addWidget(self.__speed_label)
        hb3.addWidget(self.__speed_la)
        hb2.addStretch(1)

        x_num_la = QLabel('X number:')
        self.__x_num_la_show = QLabel()
        self.__x_num_la_show.setText(str(self.__num_x - 1))
        hb12 = QHBoxLayout()
        hb12.addWidget(x_num_la)
        hb12.addWidget(self.__x_num_la_show)
        hb12.addStretch(1)

        self.__x_num_sl = QSlider(Qt.Horizontal, self)
        self.__x_num_sl.setMaximum(100)
        self.__x_num_sl.setMinimum(1)
        self.__x_num_sl.setValue(self.__num_x - 1)
        self.__x_num_sl.valueChanged.connect(self.__setXNum)

        y_num_la = QLabel('Y number:')
        self.__y_num_la_show = QLabel()
        self.__y_num_la_show.setText(str(self.__num_y - 1))
        hb13 = QHBoxLayout()
        hb13.addWidget(y_num_la)
        hb13.addWidget(self.__y_num_la_show)
        hb13.addStretch(1)

        self.__y_num_sl = QSlider(Qt.Horizontal, self)
        self.__y_num_sl.setMinimum(1)
        self.__y_num_sl.setMaximum(100)
        self.__y_num_sl.setValue(self.__num_y - 1)
        self.__y_num_sl.valueChanged.connect(self.__setYNum)

        self.__st_btn = QPushButton('Start')
        self.__st_btn.setEnabled(True)
        hb7 = QHBoxLayout()
        hb7.addWidget(self.__st_btn)
        hb7.addStretch(1)

        self.__stop_btn = QPushButton('Stop')
        self.__stop_btn.setEnabled(True)
        hb8 = QHBoxLayout()
        hb8.addWidget(self.__stop_btn)
        hb8.addStretch(1)

        self.__pause_btn = QPushButton('Pause')
        self.__pause_btn.setEnabled(True)
        hb9 = QHBoxLayout()
        hb9.addWidget(self.__pause_btn)
        hb9.addStretch(1)

        self.__new_btn = QPushButton('New Game')
        self.__new_btn.setEnabled(True)
        hb10 = QHBoxLayout()
        hb10.addWidget(self.__new_btn)
        hb10.addStretch(1)

        self.__exit_btn = QPushButton('Exit')
        self.__exit_btn.setEnabled(True)
        hb11 = QHBoxLayout()
        hb11.addWidget(self.__exit_btn)
        hb11.addStretch(1)

        self.__new_btn.clicked.connect(self.__newGameBtnAction)
        self.__st_btn.clicked.connect(self.__stBtnAction)
        self.__stop_btn.clicked.connect(self.__stopBtnAction)
        self.__pause_btn.clicked.connect(self.__pauseBtnAction)
        self.__exit_btn.clicked.connect(self.close)

        self.__lcd = QLCDNumber()
        self.__lcd.setMinimumSize(100, 100)
        hb4 = QHBoxLayout()
        hb4.addWidget(self.__lcd)
        hb4.addStretch(1)

        vb = QVBoxLayout()
        vb.addLayout(hb1)
        vb.addLayout(hb2)
        vb.addLayout(hb4)
        vb.addStretch(1)
        vb.addLayout(hb3)
        vb.addWidget(self.__sd_slider)
        vb.addLayout(hb7)
        vb.addLayout(hb8)
        vb.addLayout(hb9)
        vb.addStretch(1)
        vb.addLayout(hb12)
        vb.addWidget(self.__x_num_sl)
        vb.addLayout(hb13)
        vb.addWidget(self.__y_num_sl)
        vb.addLayout(hb10)
        vb.addStretch(10)
        vb.addLayout(hb11)

        hb5 = QHBoxLayout()
        hb5.addStretch(1)
        hb5.addLayout(vb)

        self.setLayout(hb5)
        screen = QDesktopWidget().screenGeometry()
        width = screen.width() * self.__FACTOR_SCREEN
        height = screen.height() * self.__FACTOR_SCREEN
        x0 = screen.width() * (1 - self.__FACTOR_SCREEN) / 2
        y0 = screen.height() * (1 - self.__FACTOR_SCREEN) / 2

        self.setGeometry(x0, y0, width, height)
        self.__canva_w = self.geometry().width() * self.__FACTOR
        self.__canva_h = self.geometry().height()
        self.__szx = int(self.__canva_w / self.__num_x)
        self.__szy = int(self.__canva_h / self.__num_y)

        self.setWindowTitle("Russian Block")
        self.setWindowIcon(QIcon('example.png'))
        self.show()

    # 绘制网格
    def __drawNetGrid(self, qp):
        pen = QPen(Qt.lightGray, 1, Qt.DashLine)
        qp.setPen(pen)
        for i in range(self.__num_y):
            qp.drawLine(int(self.__szx / 2), int(i * self.__szy + self.__szy / 2),
                        int(self.__num_x * self.__szx - self.__szx / 2), int(i * self.__szy + self.__szy / 2))
        for i in range(self.__num_x):
            qp.drawLine(int(i * self.__szx + self.__szx / 2), int(self.__szy / 2),
                        int(i * self.__szx + self.__szx / 2),
                        int(self.__num_y * self.__szy - self.__szy / 2))

    # 提示Game Over
    def __gameOver(self, qp, x, y):
        pen = QPen(Qt.red)
        qp.setPen(pen)
        qp.setFont(QFont('Blackoak Std', 20))
        qp.drawText(x, y, self.__canva_w / 2, self.__canva_h / 2, True, 'Game Over！')

    # 类的自调用painter绘制函数
    def paintEvent(self, e):
        self.__canvas_w = self.geometry().width() * self.__FACTOR
        self.__canvas_h = self.geometry().height()
        self.__szx = int(self.__canvas_w / self.__num_x)
        self.__szy = int(self.__canvas_h / self.__num_y)

        qp = QPainter()
        qp.begin(self)
        self.__drawNetGrid(qp)  # 绘制网格
        # 绘制形状
        for i, eles in enumerate(self.__net):
            for j, ele in enumerate(eles):
                if not ele == 0:
                    self.__drawRect(qp, j + 1, i + 1, self.__szx, self.__szy, ele)
        if self.__timer.isActive():
            self.__drawBlock(qp, self.__block, self.__szx, self.__szy)

        # game over
        if self.__gameOverFlag:
            self.__gameOverFlag = False
            self.__gameOver(qp, self.__canva_w / 4, self.__canva_h / 2)
        qp.end()

    # timer
    def timerEvent(self, e):
        if self.__isNextPosEmpty(self.__block, 0, 1):
            self.__moveBlock(0, 1)
        else:
            self.__refreshFullNet(self.__block)
            for k, ele in enumerate(self.__net):
                if 0 not in ele:
                    self.__score += 1
                    self.__level += int(self.__score / 10)
                    self.__update_score()
                    self.__update_level()
                    for i in range(k):
                        self.__net[k - i] = self.__net[k - 1 - i]
                    self.__net[0] = [0 for i in range(self.__num_x - 1)]

            # 游戏结束
            if sum([1 for ele in self.__net[0] if not ele == 0]) > 0:
                self.stop()
                self.__st_btn.setEnabled(False)
                self.__pause_btn.setEnabled(False)
                self.__stop_btn.setEnabled(False)
                self.__gameOverFlag = True
            else:
                self.__block = self.__generateRandomBlock()
        self.update()

    # 键盘按键事件
    def keyPressEvent(self, e):
        key = e.key()
        x, y = self.__block.getXY()
        if key == Qt.Key_Left:
            if (x > 1) & self.__isNextPosEmpty(self.__block, -1, 0):
                self.__block.setXY(x - 1, y)
        elif key == Qt.Key_Right:
            if self.__isNextPosEmpty(self.__block, +1, 0):
                self.__block.setXY(x + 1, y)
        elif key == Qt.Key_Down:
            if self.__isNextPosEmpty(self.__block, 0, 2):
                self.__block.setXY(x, y + 2)
        elif key == Qt.Key_Up:
            block = Block(self.__block.getXY()[0], self.__block.getXY()[1], self.__block.getShape(), self.__mshape,
                          self.__block.getColor())
            block.rota90()
            if (block.getDownBoun() > self.__num_y - 1) | (block.getLeftBoun() < 1) | (
                    block.getRightBoun() > self.__num_x - 1):
                pass
            else:
                self.__block.rota90()
        elif key == Qt.Key_P:
            if self.__timer.isActive():
                self.stop()
            else:
                self.start()
        self.update()

    # 窗口大小改变自动调用事件
    def resizeEvent(self, e):
        self.update()

    # 判占位列表是否空
    def __isNextPosEmpty(self, block, step_x, step_y):
        bot = block.getDownBoun()
        right = block.getRightBoun()
        if ((bot + step_y) > self.__num_y - 1) | ((step_x > 0) & ((right + step_x) > self.__num_x - 1)):
            return False
        pos = block.getPos()
        for p in pos:
            if p[1] < 1:
                pass
            elif not self.__net[p[1] - 1 + step_y][p[0] - 1 + step_x] == 0:
                return False
        return True

    # 更新占位列表
    def __refreshFullNet(self, block):
        for pos in block.getPos():
            if (pos[0] < 1) | (pos[1] < 1) | (pos[0] > self.__num_x - 1) | (pos[1] > self.__num_y - 1):
                pass
            self.__net[pos[1] - 1][pos[0] - 1] = block.getColor()

    # 生成一个随机对象
    def __generateRandomBlock(self):
        num_sha = random.randint(0, self.__mshape.num - 1)
        sha = self.__mshape.name[num_sha]
        num_col = random.randint(0, self.__mshape.num_col - 1)
        color = self.__mshape.color[num_col]

        x = random.randint(1, self.__num_x)
        block = Block(x, 1, sha, self.__mshape, color)
        while block.getRightBoun() > (self.__num_x - 1):
            x = random.randint(1, self.__num_x)
            block = Block(x, 1, sha, self.__mshape, color)
        return block

    # 绘制方块
    def __drawRect(self, qp, x, y, szx, szy, color):
        x_loca = x * szx - szx / 2
        y_loca = y * szy - szy / 2
        # Brush
        brush = QBrush(color)
        brush.setStyle(Qt.SolidPattern)
        qp.setBrush(brush)
        qp.drawRect(x_loca, y_loca, szx, szy)
        # Pen
        pen = QPen(Qt.darkBlue, 2, Qt.SolidLine)
        qp.setPen(pen)
        qp.drawRect(x_loca, y_loca, szx, szy)

    # 绘制特定形状
    def __drawBlock(self, qp, block, szx, szy):
        color = block.getColor()
        pos = block.getPos()
        x1 = pos[0][0]
        y1 = pos[0][1]
        x2 = pos[1][0]
        y2 = pos[1][1]
        x3 = pos[2][0]
        y3 = pos[2][1]
        x4 = pos[3][0]
        y4 = pos[3][1]
        self.__drawRect(qp, x1, y1, szx, szy, color)
        self.__drawRect(qp, x2, y2, szx, szy, color)
        self.__drawRect(qp, x3, y3, szx, szy, color)
        self.__drawRect(qp, x4, y4, szx, szy, color)

    # 移动
    def __moveBlock(self, speed_x, speed_y):
        self.__block.setXY(self.__block.getXY()[0] + speed_x, self.__block.getXY()[1] + speed_y)

    # 更新成绩
    def __update_score(self):
        self.__score_la.setText(str(self.__score))
        self.__lcd.display(str(self.__score))

    # 更新等级
    def __update_level(self):
        self.__level_la.setText(str(self.__level))

    # 滑动条事件
    def __LineEdt(self):
        self.__speed_la.setText(str(self.__sd_slider.value()))
        self.__time_step = 1010 - self.__sd_slider.value() * 10
        if self.__stop_btn.isEnabled() & self.__pause_btn.isEnabled():
            self.start()

    # 设置Xnum
    def __setXNum(self):
        self.stop()
        self.__st_btn.setEnabled(False)
        self.__stop_btn.setEnabled(False)
        self.__pause_btn.setEnabled(False)

        self.__x_num_la_show.setText(str(self.__x_num_sl.value()))
        self.__num_x = self.__x_num_sl.value()

    # 设置Y Num
    def __setYNum(self):
        self.stop()
        self.__st_btn.setEnabled(False)
        self.__stop_btn.setEnabled(False)
        self.__pause_btn.setEnabled(False)

        self.__y_num_la_show.setText(str(self.__y_num_sl.value()))
        self.__num_y = self.__y_num_sl.value()

    # 开始按钮事件
    def __stBtnAction(self):
        if self.__timer.isActive():
            pass
        else:
            self.__st_btn.setEnabled(False)
            self.__stop_btn.setEnabled(True)
            self.__pause_btn.setEnabled(True)
            self.__timer.start(self.__time_step, self)

    # 停止按钮事件
    def __stopBtnAction(self):
        if self.__timer.isActive():
            self.__timer.stop()
        self.__st_btn.setEnabled(False)
        self.__pause_btn.setEnabled(False)
        self.__stop_btn.setEnabled(False)
        self.__timer.stop()

    # 暂停按钮事件
    def __pauseBtnAction(self):
        if self.__timer.isActive():
            self.__timer.stop()
        self.__st_btn.setEnabled(True)
        self.__pause_btn.setEnabled(False)
        self.__stop_btn.setEnabled(True)

    # 新游戏按钮事件
    def __newGameBtnAction(self):
        if self.__timer.isActive():
            self.stop()
        self.__initPara()
        self.__initNet()
        self.__st_btn.setEnabled(True)
        self.__pause_btn.setEnabled(True)
        self.__stop_btn.setEnabled(True)
        self.update()
        self.start()

    # 启动时间循环时间
    def start(self):
        self.__timer.start(self.__time_step, self)

    # 停止计时器
    def stop(self):
        self.__timer.stop()


# 对象类
class Block:
    def __init__(self, x, y, shape, mshape, color):
        self.__x = x
        self.__y = y
        self.__color = color
        self.__shape = shape
        self.__sha = mshape

    # 返回四个块的中心坐标
    def getPos(self):
        x1 = x2 = x3 = x4 = self.__x
        y1 = y2 = y3 = y4 = self.__y
        if self.__shape[0] == self.__sha.hL[0]:
            x2 = x1 + 1
            x3 = x2 + 1
            x4 = x3 + 1
        elif self.__shape[0] == self.__sha.vL[0]:
            y2 = y1 + 1
            y3 = y2 + 1
            y4 = y3 + 1
        elif self.__shape[0] == self.__sha.S[0]:
            y2 = y1 + 1
            x3 = x1 + 1
            x4 = x3
            y4 = y2
        elif self.__shape[0] == self.__sha.lZ[0]:
            x2 = x1 + 1
            x3 = x2
            y3 = y2 + 1
            x4 = x3 + 1
            y4 = y3
        elif self.__shape[0] == self.__sha.ruZ[0]:
            y2 = y1 + 1
            x3 = x1 + 1
            y3 = y1 - 1
            x4 = x1 + 1
        elif self.__shape[0] == self.__sha.luZ[0]:
            y2 = y1 + 1
            x3 = x1 + 1
            y3 = y2
            x4 = x3
            y4 = y3 + 1
        elif self.__shape[0] == self.__sha.rZ[0]:
            x2 = x1 + 1
            y2 = y1 - 1
            x3 = x2
            y3 = y2 + 1
            x4 = x3 + 1
            y4 = y2
        elif self.__shape[0] == self.__sha.lvuF[0]:
            x2 = x1 + 1
            x3 = x4 = x2
            y3 = y2 + 1
            y4 = y3 + 1
        elif self.__shape[0] == self.__sha.rvuF[0]:
            x2 = x1 + 1
            y3 = y2 + 1
            y4 = y3 + 1
        elif self.__shape[0] == self.__sha.lhdF[0]:
            y2 = y1 + 1
            x3 = x1 + 1
            x4 = x3 + 1
        elif self.__shape[0] == self.__sha.rhdF[0]:
            x2 = x1 + 1
            x3 = x2 + 1
            x4 = x3
            y4 = y3 + 1
        elif self.__shape[0] == self.__sha.rvdF[0]:
            y2 = y1 + 1
            y3 = y2 + 1
            x4 = x3 + 1
            y4 = y3
        elif self.__shape[0] == self.__sha.lvdF[0]:
            x2 = x1 + 1
            y2 = y1 - 2
            x3 = x4 = x2
            y3 = y1 - 1
            y4 = y1
        elif self.__shape[0] == self.__sha.rhuF[0]:
            x2 = x1 + 1
            x3 = x2 + 1
            x4 = x3
            y4 = y3 - 1
        elif self.__shape[0] == self.__sha.lhuF[0]:
            y2 = y1 + 1
            x3 = x2 + 1
            x4 = x3 + 1
            y3 = y4 = y2
        elif self.__shape[0] == self.__sha.uW[0]:
            x2 = x1 + 1
            x3 = x2
            x4 = x3 + 1
            y2 = y1 - 1
        elif self.__shape[0] == self.__sha.dW[0]:
            x2 = x1 + 1
            x3 = x2
            x4 = x3 + 1
            y3 = y2 + 1
        elif self.__shape[0] == self.__sha.lW[0]:
            x2 = x1 + 1
            x4 = x3 = x2
            y2 = y1 - 1
            y3 = y2 + 1
            y4 = y3 + 1
        elif self.__shape[0] == self.__sha.rW[0]:
            y2 = y1 + 1
            y3 = y2 + 1
            y4 = y2
            x4 = x1 + 1
        return [(x1, y1), (x2, y2), (x3, y3), (x4, y4)]

    # 返回边界
    def getLeftBoun(self):
        return self.__x - self.__shape[1]

    def getRightBoun(self):
        return self.__x + self.__shape[2]

    def getTopBoun(self):
        return self.__y - self.__shape[3]

    def getDownBoun(self):
        return self.__y + self.__shape[4]

    # 返回形状
    def getShape(self):
        return self.__shape

    # 返回颜色
    def getColor(self):
        return self.__color

    # 设置颜色
    def setColor(self, color):
        self.__color = color

    # 设置形状
    def setShape(self, shape):
        self.__shape = shape

    # 设置坐标
    def setXY(self, x, y):
        self.__x = x
        self.__y = y

    # 返回坐标
    def getXY(self):
        return [self.__x, self.__y]

    # 移动坐标
    def __movePos(self, step_x, step_y):
        self.setXY(self.__x + step_x, self.__y + step_y)

    # 旋转90度
    def rota90(self):
        # type1
        if self.__shape[-1] == self.__sha.type1:
            if self.__shape[0] == self.__sha.vL[0]:
                self.setShape(self.__sha.hL)
                self.__movePos(-1, 1)
            elif self.__shape[0] == self.__sha.hL[0]:
                self.setShape(self.__sha.vL)
                self.__movePos(1, -1)
        # type2
        elif self.__shape[-1] == self.__sha.type2:
            pass
        # type3
        elif self.__shape[-1] == self.__sha.type3:
            if self.__shape[0] == self.__sha.lZ[0]:
                self.setShape(self.__sha.ruZ)
            elif self.__shape[0] == self.__sha.rZ[0]:
                self.setShape(self.__sha.luZ)
                self.__movePos(0, -1)
            elif self.__shape[0] == self.__sha.luZ[0]:
                self.setShape(self.__sha.rZ)
                self.__movePos(-1, 1)
            elif self.__shape[0] == self.__sha.ruZ[0]:
                self.setShape(self.__sha.lZ)
                self.__movePos(0, -1)
        # type4
        elif self.__shape[-1] == self.__sha.type4:
            if self.__shape[0] == self.__sha.lvuF[0]:
                self.setShape(self.__sha.rhuF)
                self.__movePos(0, 1)
            elif self.__shape[0] == self.__sha.lvdF[0]:
                self.setShape(self.__sha.lhuF)
                self.__movePos(0, -2)
            elif self.__shape[0] == self.__sha.rvuF[0]:
                self.setShape(self.__sha.rhdF)
                self.__movePos(-1, 1)
            elif self.__shape[0] == self.__sha.rvdF[0]:
                self.setShape(self.__sha.lhdF)
                self.__movePos(-1, 1)
            elif self.__shape[0] == self.__sha.lhuF[0]:
                self.setShape(self.__sha.rvuF)
                self.__movePos(1, 0)
            elif self.__shape[0] == self.__sha.lhdF[0]:
                self.setShape(self.__sha.lvuF)
                self.__movePos(0, -1)
            elif self.__shape[0] == self.__sha.rhuF[0]:
                self.setShape(self.__sha.rvdF)
                self.__movePos(1, -1)
            elif self.__shape[0] == self.__sha.rhdF[0]:
                self.setShape(self.__sha.lvdF)
                self.__movePos(0, 1)
        # type5
        elif self.__shape[-1] == self.__sha.type5:
            if self.__shape[0] == self.__sha.uW[0]:
                self.setShape(self.__sha.rW)
                self.__movePos(1, -1)
            elif self.__shape[0] == self.__sha.dW[0]:
                self.setShape(self.__sha.lW)
            elif self.__shape[0] == self.__sha.lW[0]:
                self.setShape(self.__sha.uW)
            elif self.__shape[0] == self.__sha.rW[0]:
                self.setShape(self.__sha.dW)
                self.__movePos(-1, +1)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    game = Game()
    sys.exit(app.exec_())
