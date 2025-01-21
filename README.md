# MATLAB-Image-Read
One piece of code done as an exam question for a MATLAB class
function [FIM] = EX2P5SSA(COSF)
%Annamaneni, Shiva

% Load the image
I = imread('Zone720.png');  % Assuming the image is in the current directory
I = double(I);  % Convert to double precision for processing

% Get the image size
[Ny, Nx] = size(I);

% Field of view dimensions in cm
FOV_x = 80;  % in cm
FOV_y = 60;  % in cm

% Calculate the corresponding spatial frequencies (in cm^-1)
fx = (0:Nx-1) / FOV_x;  % Frequency components in x-direction (cm^-1)
fy = (0:Ny-1) / FOV_y;  % Frequency components in y-direction (cm^-1)

% Create the 2D meshgrid of frequency components
[FX, FY] = meshgrid(fx, fy);

% Compute the distance from the origin in the frequency domain
D = sqrt(FX.^2 + FY.^2);

% Design the low-pass filter using fspecial
% Apply a Gaussian low-pass filter in the spatial domain
filter_size = 30; % Size of the Gaussian filter (adjust this to your needs)
sigma = 1 / COSF;  % Standard deviation related to the cutoff frequency (COSF)
h = fspecial('gaussian', filter_size, sigma);  % Create a Gaussian filter

% Apply the filter to the image using imfilter
FIM = imfilter(I, h, 'replicate');  % 'replicate' handles border effects

% Plot the original and filtered images
figure;

% Plot the original image
subplot(1, 2, 1);
imshow(I, []);  % Display original image
title('Original Image');
colormap('gray');

% Plot the filtered image
subplot(1, 2, 2);
imshow(FIM, []);  % Display filtered image
title(['Filtered Image (COSF = ', num2str(COSF), ' cm^{-1})']);
colormap('gray');

end
