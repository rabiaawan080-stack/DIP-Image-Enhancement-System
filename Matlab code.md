%% 1 IMAGE ACQUISITION
img = imread('/MATLAB Drive/im3.jfif');

if size(img,3) == 3
    gray = rgb2gray(img);
else
    gray = img;
end

figure('Name','Original Image');
imshow(gray); title('Original Grayscale Image');
saveas(gcf, 'output_original.png');

%% 6.2 SAMPLING & QUANTIZATION
scales = [0.5, 0.25, 1.5, 2];
sampled_images = cell(1,4);
figure('Name','Sampling Comparison');
for i = 1:4
    sampled_images{i} = imresize(gray, scales(i));
    subplot(2,2,i);
    imshow(sampled_images{i});
    title(sprintf('Scale = %.2f', scales(i)));
end
sgtitle('Effect of Resampling');
saveas(gcf, 'output_sampling.png');

q4 = floor(double(gray)/16) * 16;
q2 = floor(double(gray)/64) * 64;

figure('Name','Quantization Comparison');
subplot(1,3,1); imshow(gray); title('8-bit (original)');
subplot(1,3,2); imshow(uint8(q4)); title('4-bit');
subplot(1,3,3); imshow(uint8(q2)); title('2-bit');
sgtitle('Effect of Bit Depth Reduction');
saveas(gcf, 'output_quantization.png');

%% 6.3 GEOMETRIC TRANSFORMATIONS
angles = [30,45,60,90,120,150,180];
rotated = cell(1,7);
figure('Name','Rotations');
for i = 1:7
    rotated{i} = imrotate(gray, angles(i), 'bilinear', 'crop');
    subplot(2,4,i);
    imshow(rotated{i});
    title(sprintf('%d°', angles(i)));
end
sgtitle('Image Rotation at Different Angles');
saveas(gcf, 'output_rotation.png');

translation = imtranslate(gray, [50, 30], 'FillValues', 0);
tform_shear = affine2d([1 0.5 0; 0 1 0; 0 0 1]);
sheared = imwarp(gray, tform_shear);

tform_inv = affine2d(inv(tform_shear.T));
restored_shear = imwarp(sheared, tform_inv);
restored_trans = imtranslate(translation, [-50, -30], 'FillValues', 0);

figure('Name','Transformations + Inverse');
subplot(2,3,1); imshow(gray); title('Original');
subplot(2,3,2); imshow(translation); title('Translation (50,30)');
subplot(2,3,3); imshow(restored_trans); title('Restored Translation');
subplot(2,3,4); imshow(sheared); title('Shearing');
subplot(2,3,5); imshow(restored_shear); title('Restored Shearing');
subplot(2,3,6); imshow(imrotate(gray,45,'crop')); title('45° Rotation');
sgtitle('Geometric Transformations & Inverse Attempt');
saveas(gcf, 'output_geometric.png');

%% 6.4 INTENSITY TRANSFORMATIONS
neg = imcomplement(gray);
log_img = uint8(255 * mat2gray(log(1+double(gray))));
gamma_05 = imadjust(gray, [], [], 0.5);
gamma_15 = imadjust(gray, [], [], 1.5);

figure('Name','Intensity Transformations');
subplot(2,3,1); imshow(gray); title('Original');
subplot(2,3,2); imshow(neg); title('Negative');
subplot(2,3,3); imshow(log_img); title('Log');
subplot(2,3,4); imshow(gamma_05); title('Gamma 0.5');
subplot(2,3,5); imshow(gamma_15); title('Gamma 1.5');
sgtitle('Intensity Transformations');
saveas(gcf, 'output_intensity.png');

%% 6.5 HISTOGRAM PROCESSING
figure('Name','Original Histogram');
subplot(1,2,1); imshow(gray); title('Original');
subplot(1,2,2); imhist(gray); title('Histogram');
xlim([0 255]); grid on;
saveas(gcf, 'output_hist_original.png');

eq = histeq(gray);
figure('Name','Histogram Equalization');
subplot(2,2,1); imshow(gray); title('Original');
subplot(2,2,2); imhist(gray); title('Original Hist');
subplot(2,2,3); imshow(eq); title('Equalized');
subplot(2,2,4); imhist(eq); title('Equalized Hist');
sgtitle('Effect of Histogram Equalization');
saveas(gcf, 'output_histeq.png');

%% 6.6 FINAL INTEGRATED SYSTEM
function enhanced = process_image(input_image)
    gamma = imadjust(input_image, [], [], 0.5);
    enhanced = histeq(gamma);
end

final_img = process_image(gray);

figure('Name','Final Output');
subplot(1,2,1); imshow(gray); title('Original');
subplot(1,2,2); imshow(final_img); title('Final Enhanced Image');
sgtitle('Smart Image Enhancement System');
saveas(gcf, 'output_final.png');

imwrite(final_img, 'final_enhanced.jpg');
