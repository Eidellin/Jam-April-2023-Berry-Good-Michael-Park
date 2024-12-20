# BerryGood_COMP9417_Group

- target 1 : *assessment*
  - The BI-RADS assessment categories range from 0 to 6, with each category indicating a specific level of suspicion for malignancy (presence of cancer). Here is a brief overview of the BI-RADS categories:
  - Assessment 0: Incomplete – Additional imaging evaluation and/or comparison to prior mammograms is needed.
  - Assessment 1: Negative – No abnormalities were found in the breast tissue.
  - Assessment 2: Benign – A non-cancerous finding is seen in the breast tissue, and there is no need for immediate follow-up.
  - Assessment 3: Probably Benign – A finding is seen that has a low suspicion of malignancy (less than 2% risk), and a short-term follow-up is recommended.
  - Assessment 4: Suspicious Abnormality – A finding with a moderate suspicion of malignancy, warranting a biopsy or further diagnostic tests.
  - Assessment 5: Highly Suggestive of Malignancy – A finding with a high likelihood of cancer (greater than 95% chance), and appropriate action should be taken.
  - Assessment 6: Known Biopsy-Proven Malignancy – A finding that has been previously confirmed as malignant through a biopsy, and the patient is awaiting treatment.
    (Not Applicable to our dataset)
  
- target 2 : *pathology* (model generated using assessment + other features)
  - Benign without callback - no need for immediate follow-up
  - Benign - non-cancerous but follow-up recomended 
  - Malignant - cancerous

## Explanations of files and how to run each model

I removed except my model (FNN).
  
- FNN.py:
  - The code has been tested with python 3.9
  - When you start the code, there will be input "How many iteration for each parameter tuning? (default 50) "
    - If you give the integer value then that would be the number of iteration for each hyperparameter tuning approach.
    - Otherwise, default value will be kept
  - Then each model will be trained (including hyperparameter tuning by the code itself).
  - In each execution, the result would be different but generally with greater iteration, meaning number of training model with each parameter to tune the hyperparameter is more likely with the better result.
  - Trained models will be saved on the same path as this code and its name will be printed with its accuracy.
  - Save/Load state_dict (Recommended) Method was used from 
    - https://pytorch.org/tutorials/beginner/saving_loading_models.html#saving-loading-model-for-inference
    - hence, please read this document if you want to load the model.
    
    
    - If you want to re-evaluate those saved model, you can see FNN_evaluate.py file which evaluate the provided trained model. (Will be submitted with the report at Monday).
    - if you got error like this:
      - Traceback (most recent call last):
          File "C:\UNI\UNSW2023T1\COMP9417\Group Project\breast cancer\FNN_evaluate.py", line 257, in <module>
            model.load_state_dict(torch.load("FNN_fpc.pt"))
          File "C:\Users\sonog\AppData\Local\Programs\Python\Python39\lib\site-packages\torch\nn\modules\module.py", line 1671, in load_state_dict
            raise RuntimeError('Error(s) in loading state_dict for {}:\n\t{}'.format(
        RuntimeError: Error(s) in loading state_dict for FNN:
                Unexpected key(s) in state_dict: "linear_non_linear_stack.6.weight", "linear_non_linear_stack.6.bias".
                size mismatch for linear_non_linear_stack.4.weight: copying a param with shape torch.Size([512, 512]) from checkpoint, the shape in current model is torch.Size([3, 512]).
                size mismatch for linear_non_linear_stack.4.bias: copying a param with shape torch.Size([512]) from checkpoint, the shape in current model is torch.Size([3]).
      - That is because of the hidden_layers parameter for FNN. Then, uncomment two lines for the model which caused that error, it looks like, for example, 
        for key in torch.load("FNN_fpc.pt").keys():
          print(f"{key} : ", torch.load("FNN_fpc.pt")[key].shape)
      - Then change hidden_layers.
      - Caution: FNN_evaluate.py is evaluating for the provided trained model. You need to change the name of file paths. This is the guide if you want to test the saved model, it does not evaluate your 'any' saved model.
      - Those saved models are "FNN_fac.pt", "FNN_fam.pt", "FNN_fpc.pt" and "FNN_fpm.pt".

## Results
| Task                 | Random Forest | KNN    | Decision Tree | LightGBM | FNN    | Best       |
|----------------------|---------------|--------|---------------|----------|--------|------------|
| Calc-Assessment      | 0.6841        | 0.6643 | 0.6714        | 0.7004   | 0.7730 | FNN        |
| Mass-Assessment      | 0.5344        | 0.6005 | 0.6031        | 0.6402   | 0.6402 | FNN/LightGBM |
| Calc-Pathology       | 0.7383        | 0.6986 | 0.7509        | 0.7220   | 0.7546 | FNN        |
| Mass-Pathology       | 0.7592        | 0.7169 | 0.7460        | 0.7751   | 0.7381 | LightGBM   |

## Links:
 - [desc of dataset]{https://www.nature.com/articles/sdata2017177#Sec13}
 - [dataset]{https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=22516629#22516629e30c416b9d7e4e2aa42b617c35433a6b}
 - [image processing library]{https://github.com/fjeg/ddsm_tools/tree/master/ddsm_tools}  