Matlab代码

~~~matlab
%p7.m
%直线检测
test1=im2double(imread('test1.tif'));
test1=test1(:,:,1);
test2=im2double(imread('test2.png'));
test3=im2double(imread('test3.jpg'));
test4=im2double(imread('test4.bmp'));
test5=im2double(imread('test5.png'));
test6=im2double(imread('test6.jpg'));
NumPeaks=10;
t=0.5;
dtheta=10;
drho=10;

%Sobel
for i=1:6
    test=['test',num2str(i)];
    eval(['test_s=edge(',test,',''sobel'');;']);
    figure(1)
    subplot(2,3,i)
    imshow(test_s);
    title(test);
    figure(2)
    subplot(2,3,i)
    HoughTransform(test_s,dtheta,drho,NumPeaks,t);
    title(test);
end

%Canny
for i=1:6
    test=['test',num2str(i)];
    eval(['test_c=edge(',test,',''canny'',[0.1,0.2],1.5);;']);
    figure(3)
    subplot(2,3,i)
    imshow(test_c);
    title(test);
    figure(4)
    subplot(2,3,i)
    HoughTransform(test_c,dtheta,drho,NumPeaks,t);
    title(test);
end

~~~

~~~matlab
function []=HoughTransform(im_in,dtheta,drho,NumPeaks,t)
%Hough变换
[H,theta,rho]=hough(im_in,'RhoResolution',drho,'ThetaResolution',dtheta);
% imshow(H,[],'XData',theta,'YData',rho,'InitialMagnification','fit');
% axis on,axis,normal
% xlabel('\theta'),ylabel('\rho')
peaks=houghpeaks(H,NumPeaks,'Threshold',t*max(H(:)));
lines=houghlines(im_in,theta,rho,peaks);
imshow(im_in);
hold on
for k=1:length(lines)
    xy=[lines(k).point1;lines(k).point2];
    plot(xy(:,1),xy(:,2),'LineWidth',2,'Color','r');
end
~~~

