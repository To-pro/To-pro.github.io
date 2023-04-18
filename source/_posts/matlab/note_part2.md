---
title: Matlab figure 筆記2
cover: matlab/cover.jpg
# # 或者写成
# cover: http://placehold.it/350x150.jpg
categories: 
- [matlab]
---
### 示範在空間中畫一個方塊。並運用方向餘弦矩陣(DCM)，畫不同姿態的方塊
重點函數說明:
[patch](https://www.mathworks.com/help/matlab/ref/patch.html) 畫方塊

[quiver3](https://www.mathworks.com/help/matlab/ref/quiver3.html) 畫箭頭

[angle2dcm](https://www.mathworks.com/help/aerotbx/ug/angle2dcm.html) 產生方向餘弦矩陣來旋轉方塊(預設為(yaw,pitch,roll))

[drawnow](https://www.mathworks.com/help/matlab/ref/drawnow.html) 更新figure
```MATLAB
clc;clear;close all;

fig(1) = figure("Name","cube");
vertices = [-0.5 -0.5 -0.5;
             0.5 -0.5 -0.5;
             0.5  0.5 -0.5;
            -0.5  0.5 -0.5;
            -0.5 -0.5  0.5;
             0.5 -0.5  0.5;
             0.5  0.5  0.5;
            -0.5  0.5  0.5];%正方體有八個頂點
faces = [1 2 6 5;2 3 7 6;3 4 8 7;4 1 5 8;1 2 3 4;5 6 7 8];%其6個面的頂點，點的順序根據vertices
cube = patch('Vertices',vertices,'Faces',faces,'Facecolor','none');
Vx_ini = [1 0 0]';
Vy_ini = [0 1 0]';
Vz_ini = [0 0 1]';
Vx = [1 0 0]';
Vy = [0 1 0]';
Vz = [0 0 1]';
hold on
Xaxis = quiver3(0,0,0,Vx(1),Vx(2),Vx(3),1.5,'r','filled','Linewidth',2);
Yaxis = quiver3(0,0,0,Vy(1),Vy(2),Vy(3),1.5,'g','filled','Linewidth',2);
Zaxis = quiver3(0,0,0,Vz(1),Vz(2),Vz(3),1.5,'b','filled','Linewidth',2);
ax=gca;
ax.FontSize = 14;
ax.FontName = 'Times New Roman';
grid on %開網格
view(3) %3D來看
axis equal  %沿每個軸對數據單位使用相同的長度。
axis([-1.5 2.2 -1.5 2.2 -1.5 2.2]);

n = 7;
for i = 1:n
dcm = angle2dcm (pi/4*(i-1),0,0);%旋轉餘弦矩陣

cube.Vertices = vertices*dcm;
Vx=dcm'*Vx_ini;
Vy=dcm'*Vy_ini;
Vz=dcm'*Vz_ini;
Xaxis.UData=Vx(1);Xaxis.VData=Vx(2);Xaxis.WData=Vx(3);
Yaxis.UData=Vy(1);Yaxis.VData=Vy(2);Yaxis.WData=Vy(3);
Zaxis.UData=Vz(1);Zaxis.VData=Vz(2);Zaxis.WData=Vz(3);

drawnow;%以新的姿態重新繪製方塊
if(i<n)
    fprintf("Press any key to continue.\n");
    pause;%暫停直到按任意鍵
else
    fprintf("Rotation display ends.\n");
end

end
```
圖:
![Imgur](https://imgur.com/dTdj4Zz.jpg)