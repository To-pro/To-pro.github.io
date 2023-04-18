---
title: Matlab figure 筆記1
categories: 
- [matlab]
---

繪圖的精神為清晰明顯的表達所有資訊
---
此次所用資料生成
假設有如下生成的資料需要繪圖
``` matlab
clc;clear;close all;
addpath 'Z:\My Drive\matlab\plot_function'%本人自訂函數存放處
dt = 0.001;
t_end = 10;
t = 0:dt:t_end;
theta = (1-exp(-t))+exp(-0.6*t).*0.1.*sin(10*t);
noise = 0.01*randn(1,length(t));
theta_m = theta + noise;
```
基本的繪圖
---
基本上繪圖要有
>X座標(含單位)
>Y座標(含單位)
>圖例(當此圖有多條線時)
>標題(可有可無，可在報告中的標號時說明)

其中 [plot](https://www.mathworks.com/help/matlab/ref/plot.html) (建議多參考官網的說明)有幾個重要屬性基本都要調整使其繪的圖可以清晰，==LineStyle==、==Color==和==LineWidth==。
以下示範簡單繪製$\theta$與$\theta_m$在同一張圖。因為$\theta_m$含有噪音，要優先畫才不會蓋到$\theta$
``` matlab
figure("Name","state")
plot(t,theta_m,"-r");
hold on
plot(t,theta,"-b");
legend("\theta","\theta_m")%圖例
xlabel("Time (sec)","Interpreter","latex");%X座標名稱
ylabel("\theta (rad/s)");%Y座標名稱
title("Response")%標題
```
![img](https://i.imgur.com/wjzXxqV.png)
此程式碼繪的圖有幾個問題
>線粗細可以在粗一些
>圖例擋到線
>字太小(這要根據你最終要放到的報告中調整，簡單來說要與報告內文相同)
>不知道最終值是不是在1附近


高級的繪圖
---
除了剛剛的缺點，這裡還更改了圖例顯示的順序，用物件接住plot，在對此物件標示圖例，此法可以延伸應用到僅標示要標示的線(參考雙Y軸圖的程式碼)
``` matlab
figure('Name','response')
p(2)=plot(t,theta_m,'-r','LineWidth',2);
hold on
p(1)=plot(t,theta,'-b','LineWidth',2);
legend(p,'\theta_m','\theta','location','southeast')
xlabel('Time (sec)');%X座標名稱
ylabel('\theta (rad/s)');%Y座標名稱
title('Response')%標題
ax = gca;
set(ax,'FontSize',15);
set(ax,'FontName','Times New Roman');
set(ax,'XGrid','on','YGrid','on');
ax.TitleFontSizeMultiplier=1.3;
set(ax,'XMinorGrid','on','YMinorGrid','on');
```
![img](https://imgur.com/BVXeQTb.png)
小提示:每個東西都有很多屬性可以設定。令一個變數接住，並且後面不加分號。你就能在 Command Window 看到新世界(有很多屬性可以調整)。將上面程式碼中的 legend 改成像這樣
```
l = legend(p,'\theta_m','\theta')
```
繪圖函數設定
---
因為每張圖都有類似的共同的設定，因此將其包成函數，以後畫圖就很輕鬆，這是我常用的繪圖設定函數
``` matlab
function plot_set(varargin)
nInputs = nargin;%偵測輸入有幾個變數
if nInputs ==1%若只有一個變數就是字體大小的設定值
    ax_FontSize = varargin{1};
else
    ax_FontSize = varargin{1};%字體大小的設定值
    Line_width = varargin{2};%所有 plot 的線粗細設定值
end
ax = gca;%當前座標區或圖
set(ax,'FontSize',ax_FontSize);%字體大小設定
set(ax,'FontName','Times New Roman');%字體設定
set(ax,'XGrid','on','YGrid','on');%開網格
set(ax,'XMinorGrid','on','YMinorGrid','on');%開更密的網格
ax.TitleFontSizeMultiplier=1.3;%標題與一般文字大小的放大倍率

%所有 plot 的線粗細設定
if nInputs == 2
plot_num = max(size(ax.Children));%ax.Children就是figure裡plot的線
for i = 1: plot_num
%     ax.Children(i).LineStyle = '-';%統一線的樣式(通常多條線就要不一樣)
    ax.Children(i).LineWidth = Line_width;
end
end
end
``` 
則使用以下程式碼就能畫出相同的圖
``` matlab
figure("Name","state")
p(2)=plot(t,theta_m,"-r","LineWidth",2);
hold on
p(1)=plot(t,theta,"-b","LineWidth",2);
legend(p,["\theta_m" "\theta"],"location","best")
xlabel("Time (sec)","Interpreter","latex");%X座標名稱
ylabel("\theta (rad/s)");%Y座標名稱
title("Response")%標題
plot_set(15,2)
``` 
子母圖
---
有時需要放大圖中某個地方等需求，就可以這樣畫
![Imgur](https://imgur.com/Yguh8h3.png)
程式碼如下
```  matlab
figure('Name','response')
p(2)=plot(t,theta_m,'-r');
hold on
p(1)=plot(t,theta,'-b');
legend(p,'\theta_m','\theta','location','southeast')
xlabel('Time (sec)','Interpreter','latex');%X座標名稱
ylabel('\theta (rad/s)');%Y座標名稱
title('Response')%標題
plot_set(15,2)

axes('position',[0.45,0.40,0.4,0.3]);
%子圖位置與大小[距圖左邊距離 距圖下邊距離 子圖寬 子圖高]
plot(t,theta_m,'-r');
hold on
plot(t,theta,'-b');
axis([7 10 -inf inf])
plot_set(15,2)
ylim 'padded'%padded 為邊距的寬度約為數據范圍的 7%
``` 

雙Y軸圖
---
有時要繪製在一起的資料量級相差太大，但又想繪製在一起觀察，就可以考慮有效利用兩邊Y軸來繪圖
![Imgur](https://imgur.com/pZHZdZi.png)
```  matlab
figure('Name','All')
p(2)=plot(t,theta_m,'-r');
hold on
p(1)=plot(t,theta,'-b');
xlabel('Time (sec)','Interpreter','latex');%X座標名稱
ylabel('\theta (rad/s)');%左邊Y座標名稱
title('Response')%標題
plot_set(15,2)

yyaxis right %改右邊Y軸來畫
plot(t,input,'-m');
hold on
ylabel('voltage (V)','Color','m');%右邊Y座標名稱
ax = gca;
ax.YColor='m';
legend(p,'\theta_m','\theta','location','best')
ylim 'padded'%padded 為邊距的寬度約為數據范圍的 7%
plot_set(15,2)
``` 
[前後問題改起來很麻煩可以參考這裡解決](https://www.mathworks.com/matlabcentral/answers/1661295-how-to-set-front-and-back-lines-on-plot-with-yyaxis-left-and-right)

用迴圈存圖
---
最後耍帥的用程式碼將全部圖片存到指定地點，後續可用 Latex 自動讀檔製作報告，使用函數[print](https://www.mathworks.com/help/matlab/ref/print.html)
``` matlab
fig(1)=figure('Name','theta');
plot(t,theta,'-b');
xlabel('Time (sec)','Interpreter','latex');
ylabel('voltage (V)');
plot_set(15,2)

fig(end+1)=figure('Name','theta_m');
plot(t,theta_m,'-r');
xlabel('Time (sec)','Interpreter','latex');
ylabel('voltage (V)');
plot_set(15,2)

fig(end+1)=figure('Name','input');
plot(t,input,'-m');
xlabel('Time (sec)','Interpreter','latex');
ylabel('voltage (V)');
plot_set(15,2)

for i = 1:length(fig)
     print(fig(i),['Z:\My Drive\matlab\example\Figure\' fig(i).Name], '-depsc');
end  
``` 
備註:這裡設定 -depsc 是存成向量檔(.eps)，放大才不會有像素感。