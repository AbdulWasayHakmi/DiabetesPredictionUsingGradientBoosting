clc, close all;
data = readtable("diabetes.csv");
head(data);
summary(data);

%*****************TARGET DISTRIBUTION*******************%
h = sum(data.Outcome == 1);
l = sum(data.Outcome == 0);

figure;
b1 = bar([l, h]);
set(gca, 'xticklabel', {'0 (No Diabetes)', '1 (Diabetes)'});
ylabel('Count');
title('Target Distribution');
text(b1.XData(1), b1.YData(1), num2str(l), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b1.XData(2), b1.YData(2), num2str(h), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
grid on;

%*****************CORRELATION MATRIX*******************%
correlationMatrix = corrcoef(data{:,:});
disp(correlationMatrix);

figure;
heatmap(correlationMatrix, 'XData', data.Properties.VariableNames, 'YData', data.Properties.VariableNames);

%*****************NUMBER OF MISSING VALUES*******************%
numOfMissG = sum(data.Glucose == 0);
numOfMissBP = sum(data.BloodPressure == 0);
numOfMissSKT = sum(data.SkinThickness == 0);
numOfMissIN = sum(data.Insulin == 0);
numOfMissBMI = sum(data.BMI == 0);
numOfMissDPF = sum(data.DiabetesPedigreeFunction == 0);
numOfMissAGE = sum(data.Age == 0);

numOfPreg = height(data.Pregnancies);
numOfG = height(data.Glucose) - numOfMissG;
numOfBP = height(data.Glucose) - numOfMissBP;
numOfSKT = height(data.Glucose) - numOfMissSKT;
numOfIN = height(data.Glucose) - numOfMissIN;
numOfBMI = height(data.Glucose) - numOfMissBMI;
numOfDPF = height(data.Glucose) - numOfMissDPF;
numOfAGE = height(data.Glucose) - numOfMissAGE;
numofOUT = height(data.Outcome);

figure;
b = bar([numOfPreg, numOfG, numOfBP, numOfSKT, numOfIN, numOfBMI, numOfDPF, numOfAGE, numofOUT]);
set(gca, 'xticklabel', {'Pregnancies', 'Glucose', 'BloodPressure', ...
            'SkinThickness', 'Insulin', 'BMI', 'DiabetesPedigreeFunction', 'Age', 'Outcome'});
ylabel('Count');
title('Number of Missing Values');
text(b.XData(1), b.YData(1), num2str(768), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(2), b.YData(2), num2str(numOfG), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(3), b.YData(3), num2str(numOfBP), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(4), b.YData(4), num2str(numOfSKT), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(5), b.YData(5), num2str(numOfIN), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(6), b.YData(6), num2str(numOfBMI), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(7), b.YData(7), num2str(numOfDPF), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(8), b.YData(8), num2str(numOfAGE), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(9), b.YData(9), num2str(768), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
grid on;

%*****************UNIVARIATE DISTRIBUTIONS*******************%
figure;
subplot(1, 2, 1);
histogram(data.Pregnancies);
title('Pregnancy Distribution');
xlabel('Pregnancies');
ylabel('Frequency');
subplot(1, 2, 2);
boxplot(data.Pregnancies);
title('Pregnancy Distribution');
ylabel('Pregnancies');

figure;
subplot(1, 2, 1);
histogram(data.Glucose);
title('Glucose Distribution');
xlabel('Glucose');
ylabel('Frequency');
subplot(1, 2, 2);
boxplot(data.Glucose);
title('Glucose Distribution');
ylabel('Glucose');

figure;
subplot(1, 2, 1);
histogram(data.BloodPressure);
title('BloodPressure Distribution');
xlabel('BloodPressure');
ylabel('Frequency');
subplot(1, 2, 2);
boxplot(data.BloodPressure);
title('BloodPressure Distribution');
ylabel('BloodPressure');

figure;
subplot(1, 2, 1);
histogram(data.SkinThickness);
title('SkinThickness Distribution');
xlabel('SkinThickness');
ylabel('Frequency');
subplot(1, 2, 2);
boxplot(data.SkinThickness);
title('SkinThickness Distribution');
ylabel('SkinThickness');

figure;
subplot(1, 2, 1);
histogram(data.Insulin);
title('Insulin Distribution');
xlabel('Insulin');
ylabel('Frequency');
subplot(1, 2, 2);
boxplot(data.Insulin);
title('Insulin Distribution');
ylabel('Insulin');

figure;
subplot(1, 2, 1);
histogram(data.BMI);
title('BMI Distribution');
xlabel('BMI');
ylabel('Frequency');
subplot(1, 2, 2);
boxplot(data.BMI);
title('BMI Distribution');
ylabel('BMI');

figure;
subplot(1, 2, 1);
histogram(data.DiabetesPedigreeFunction);
title('DiabetesPedigreeFunction Distribution');
xlabel('DiabetesPedigreeFunction');
ylabel('Frequency');
subplot(1, 2, 2);
boxplot(data.DiabetesPedigreeFunction);
title('DiabetesPedigreeFunction Distribution');
ylabel('DiabetesPedigreeFunction');

figure;
subplot(1, 2, 1);
histogram(data.Age);
title('Age Distribution');
xlabel('Age');
ylabel('Frequency');
subplot(1, 2, 2);
boxplot(data.Age);
title('Age Distribution');
ylabel('Age');

data.Glucose(data.Glucose == 0) = NaN;
data.BloodPressure(data.BloodPressure == 0) = NaN;
data.SkinThickness(data.SkinThickness == 0) = NaN;
data.Insulin(data.Insulin == 0) = NaN;
data.BMI(data.BMI == 0) = NaN;

head(data);
summary(data);

medianGlucose = median(data.Glucose, 'omitnan'); 
data.Glucose(isnan(data.Glucose)) = medianGlucose; 

medianBloodPressure = median(data.BloodPressure, 'omitnan');
data.BloodPressure(isnan(data.BloodPressure)) = medianBloodPressure;

medianSkinThickness = median(data.SkinThickness, 'omitnan');
data.SkinThickness(isnan(data.SkinThickness)) = medianSkinThickness;

medianInsulin = median(data.Insulin, 'omitnan');
data.Insulin(isnan(data.Insulin)) = medianInsulin;

medianBMI = median(data.BMI, 'omitnan');
data.BMI(isnan(data.BMI)) = medianBMI;

head(data);
summary(data);

numOfMissG = sum(data.Glucose == 0);
numOfMissBP = sum(data.BloodPressure == 0);
numOfMissSKT = sum(data.SkinThickness == 0);
numOfMissIN = sum(data.Insulin == 0);
numOfMissBMI = sum(data.BMI == 0);
numOfMissDPF = sum(data.DiabetesPedigreeFunction == 0);
numOfMissAGE = sum(data.Age == 0);

numOfPreg = height(data.Pregnancies);
numOfG = height(data.Glucose) - numOfMissG;
numOfBP = height(data.Glucose) - numOfMissBP;
numOfSKT = height(data.Glucose) - numOfMissSKT;
numOfIN = height(data.Glucose) - numOfMissIN;
numOfBMI = height(data.Glucose) - numOfMissBMI;
numOfDPF = height(data.Glucose) - numOfMissDPF;
numOfAGE = height(data.Glucose) - numOfMissAGE;
numofOUT = height(data.Outcome);

figure;
b = bar([numOfPreg, numOfG, numOfBP, numOfSKT, numOfIN, numOfBMI, numOfDPF, numOfAGE, numofOUT]);
set(gca, 'xticklabel', {'Pregnancies', 'Glucose', 'BloodPressure', ...
            'SkinThickness', 'Insulin', 'BMI', 'DiabetesPedigreeFunction', 'Age', 'Outcome'});
ylabel('Count');
title('Number of Missing Values');
text(b.XData(1), b.YData(1), num2str(768), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(2), b.YData(2), num2str(numOfG), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(3), b.YData(3), num2str(numOfBP), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(4), b.YData(4), num2str(numOfSKT), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(5), b.YData(5), num2str(numOfIN), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(6), b.YData(6), num2str(numOfBMI), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(7), b.YData(7), num2str(numOfDPF), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(8), b.YData(8), num2str(numOfAGE), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
text(b.XData(9), b.YData(9), num2str(768), ...
    'HorizontalAlignment', 'center', 'VerticalAlignment', 'middle', 'Color', 'black');
grid on;

standardizedData = zscore(data{:, 2:end});
data{:, 2:end} = standardizedData;

head(data);

rng(1);
cv = cvpartition(height(data), 'HoldOut', 0.2);

trainData = data(training(cv), :);
testData = data(test(cv), :);

predictors = trainData{:, 1:end-1};
response = trainData.Outcome; 

learningRates = [0.01, 0.05, 0.1];
maxSplits = [4, 8, 12]; 
numCycles = [50, 100, 200];

bestAccuracy = 0;
for lr = learningRates
    for splits = maxSplits
        for cycles = numCycles
            t = templateTree('MaxNumSplits', splits);
            model = fitcensemble(predictors, response, 'Method', 'LogitBoost', ...
                                 'NumLearningCycles', cycles, 'LearnRate', lr, 'Learners', t);
            predictions = predict(model, testPredictors);
            accuracy = sum(predictions == testResponse) / length(testResponse);
            if accuracy > bestAccuracy
                bestAccuracy = accuracy;
                bestModel = model;
            end
        end
    end
end
disp(['Best Model Accuracy: ', num2str(bestAccuracy * 100), '%']);

confusionMatrix = confusionmat(testResponse, predictions);
disp(confusionMatrix);

figure;
confusionChart = confusionchart(testResponse, predictions);

title('Confusion Matrix for Diabetes Prediction');
xlabel('Predicted Class');
ylabel('True Class');