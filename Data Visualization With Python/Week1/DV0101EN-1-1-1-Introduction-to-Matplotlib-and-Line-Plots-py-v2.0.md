<a href="https://cognitiveclass.ai"><img src = "https://ibm.box.com/shared/static/9gegpsmnsoo25ikkbl4qzlvlyjbgxs5x.png" width = 400> </a>

<h1 align=center><font size = 5>Introduction to Matplotlib and Line Plots</font></h1>

## Introduction

The aim of these labs is to introduce you to data visualization with Python as concrete and as consistent as possible. 
Speaking of consistency, because there is no *best* data visualization library avaiblable for Python - up to creating these labs - we have to introduce different libraries and show their benefits when we are discussing new visualization concepts. Doing so, we hope to make students well-rounded with visualization libraries and concepts so that they are able to judge and decide on the best visualitzation technique and tool for a given problem _and_ audience.

Please make sure that you have completed the prerequisites for this course, namely <a href='http://cocl.us/PY0101EN_DV0101EN_LAB1_Coursera'>**Python for Data Science**</a> and <a href='http://cocl.us/DA0101EN_DV0101EN_LAB1_Coursera'>**Data Analysis with Python**</a>, which are part of this specialization. 

**Note**: The majority of the plots and visualizations will be generated using data stored in *pandas* dataframes. Therefore, in this lab, we provide a brief crash course on *pandas*. However, if you are interested in learning more about the *pandas* library, detailed description and explanation of how to use it and how to clean, munge, and process data stored in a *pandas* dataframe are provided in our course <a href='http://cocl.us/DA0101EN_DV0101EN_LAB1_Coursera'>**Data Analysis with Python**</a>, which is also part of this specialization. 

------------

## Table of Contents

<div class="alert alert-block alert-info" style="margin-top: 20px">

1. [Exploring Datasets with *pandas*](#0)<br>
1.1 [The Dataset: Immigration to Canada from 1980 to 2013](#2)<br>
1.2 [*pandas* Basics](#4) <br>
1.3 [*pandas* Intermediate: Indexing and Selection](#6) <br>
2. [Visualizing Data using Matplotlib](#8) <br>
2.1 [Matplotlib: Standard Python Visualization Library](#10) <br>
3. [Line Plots](#12)
</div>
<hr>

# Exploring Datasets with *pandas* <a id="0"></a>

*pandas* is an essential data analysis toolkit for Python. From their [website](http://pandas.pydata.org/):
>*pandas* is a Python package providing fast, flexible, and expressive data structures designed to make working with “relational” or “labeled” data both easy and intuitive. It aims to be the fundamental high-level building block for doing practical, **real world** data analysis in Python.

The course heavily relies on *pandas* for data wrangling, analysis, and visualization. We encourage you to spend some time and  familizare yourself with the *pandas* API Reference: http://pandas.pydata.org/pandas-docs/stable/api.html.

## The Dataset: Immigration to Canada from 1980 to 2013 <a id="2"></a>

Dataset Source: [International migration flows to and from selected countries - The 2015 revision](http://www.un.org/en/development/desa/population/migration/data/empirical2/migrationflows.shtml).

The dataset contains annual data on the flows of international immigrants as recorded by the countries of destination. The data presents both inflows and outflows according to the place of birth, citizenship or place of previous / next residence both for foreigners and nationals. The current version presents data pertaining to 45 countries.

In this lab, we will focus on the Canadian immigration data.

<img src = "https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DV0101EN/labs/Images/Mod1Fig1-Dataset.png" align="center" width=900>

For sake of simplicity, Canada's immigration data has been extracted and uploaded to one of IBM servers. You can fetch the data from [here](https://ibm.box.com/shared/static/lw190pt9zpy5bd1ptyg2aw15awomz9pu.xlsx).

---

## *pandas* Basics<a id="4"></a>

The first thing we'll do is import two key data analysis modules: *pandas* and **Numpy**.


```python
import numpy as np  # useful for many scientific computing in Python
import pandas as pd # primary data structure library
```


```python
!conda install -c anaconda xlrd --yes
```

    Solving environment: done
    
    
    ==> WARNING: A newer version of conda exists. <==
      current version: 4.5.11
      latest version: 4.8.0
    
    Please update conda by running
    
        $ conda update -n base -c defaults conda
    
    
    
    ## Package Plan ##
    
      environment location: /home/jupyterlab/conda/envs/python
    
      added / updated specs: 
        - xlrd
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        numpy-base-1.15.4          |   py36h81de0dd_0         4.2 MB  anaconda
        numpy-1.15.4               |   py36h1d66e8a_0          35 KB  anaconda
        openssl-1.1.1              |       h7b6447c_0         5.0 MB  anaconda
        mkl_fft-1.0.6              |   py36h7dd41cf_0         150 KB  anaconda
        certifi-2019.11.28         |           py36_0         156 KB  anaconda
        blas-1.0                   |              mkl           6 KB  anaconda
        scipy-1.1.0                |   py36hfa4b5c9_1        18.0 MB  anaconda
        xlrd-1.2.0                 |             py_0         108 KB  anaconda
        mkl_random-1.0.1           |   py36h4414c95_1         373 KB  anaconda
        scikit-learn-0.20.1        |   py36h4989274_0         5.7 MB  anaconda
        ------------------------------------------------------------
                                               Total:        33.7 MB
    
    The following packages will be UPDATED:
    
        certifi:      2019.9.11-py36_0                       conda-forge --> 2019.11.28-py36_0     anaconda
        mkl_fft:      1.0.4-py37h4414c95_1                               --> 1.0.6-py36h7dd41cf_0  anaconda
        mkl_random:   1.0.1-py37h4414c95_1                               --> 1.0.1-py36h4414c95_1  anaconda
        numpy-base:   1.15.1-py37h81de0dd_0                              --> 1.15.4-py36h81de0dd_0 anaconda
        openssl:      1.1.1d-h516909a_0                      conda-forge --> 1.1.1-h7b6447c_0      anaconda
        xlrd:         1.1.0-py37_1                                       --> 1.2.0-py_0            anaconda
    
    The following packages will be DOWNGRADED:
    
        blas:         1.1-openblas                           conda-forge --> 1.0-mkl               anaconda
        numpy:        1.16.2-py36_blas_openblash1522bff_0    conda-forge [blas_openblas] --> 1.15.4-py36h1d66e8a_0 anaconda
        scikit-learn: 0.20.1-py36_blas_openblashebff5e3_1200 conda-forge [blas_openblas] --> 0.20.1-py36h4989274_0 anaconda
        scipy:        1.2.1-py36_blas_openblash1522bff_0     conda-forge [blas_openblas] --> 1.1.0-py36hfa4b5c9_1  anaconda
    
    
    Downloading and Extracting Packages
    numpy-base-1.15.4    | 4.2 MB    | ##################################### | 100% 
    numpy-1.15.4         | 35 KB     | ##################################### | 100% 
    openssl-1.1.1        | 5.0 MB    | ##################################### | 100% 
    mkl_fft-1.0.6        | 150 KB    | ##################################### | 100% 
    certifi-2019.11.28   | 156 KB    | ##################################### | 100% 
    blas-1.0             | 6 KB      | ##################################### | 100% 
    scipy-1.1.0          | 18.0 MB   | ##################################### | 100% 
    xlrd-1.2.0           | 108 KB    | ##################################### | 100% 
    mkl_random-1.0.1     | 373 KB    | ##################################### | 100% 
    scikit-learn-0.20.1  | 5.7 MB    | ##################################### | 100% 
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done


Let's download and import our primary Canadian Immigration dataset using *pandas* `read_excel()` method. Normally, before we can do that, we would need to download a module which *pandas* requires to read in excel files. This module is **xlrd**. For your convenience, we have pre-installed this module, so you would not have to worry about that. Otherwise, you would need to run the following line of code to install the **xlrd** module:
```
!conda install -c anaconda xlrd --yes
```

Now we are ready to read in our data.


```python
df_can = pd.read_excel('https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DV0101EN/labs/Data_Files/Canada.xlsx',
                       sheet_name='Canada by Citizenship',
                       skiprows=range(20),
                       skipfooter=2)

print ('Data read into a pandas dataframe!')
```

    Data read into a pandas dataframe!


Let's view the top 5 rows of the dataset using the `head()` function.


```python
df_can.head()
# tip: You can specify the number of rows you'd like to see as follows: df_can.head(10) 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Coverage</th>
      <th>OdName</th>
      <th>AREA</th>
      <th>AreaName</th>
      <th>REG</th>
      <th>RegName</th>
      <th>DEV</th>
      <th>DevName</th>
      <th>1980</th>
      <th>...</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>Afghanistan</td>
      <td>935</td>
      <td>Asia</td>
      <td>5501</td>
      <td>Southern Asia</td>
      <td>902</td>
      <td>Developing regions</td>
      <td>16</td>
      <td>...</td>
      <td>2978</td>
      <td>3436</td>
      <td>3009</td>
      <td>2652</td>
      <td>2111</td>
      <td>1746</td>
      <td>1758</td>
      <td>2203</td>
      <td>2635</td>
      <td>2004</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>Albania</td>
      <td>908</td>
      <td>Europe</td>
      <td>925</td>
      <td>Southern Europe</td>
      <td>901</td>
      <td>Developed regions</td>
      <td>1</td>
      <td>...</td>
      <td>1450</td>
      <td>1223</td>
      <td>856</td>
      <td>702</td>
      <td>560</td>
      <td>716</td>
      <td>561</td>
      <td>539</td>
      <td>620</td>
      <td>603</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>Algeria</td>
      <td>903</td>
      <td>Africa</td>
      <td>912</td>
      <td>Northern Africa</td>
      <td>902</td>
      <td>Developing regions</td>
      <td>80</td>
      <td>...</td>
      <td>3616</td>
      <td>3626</td>
      <td>4807</td>
      <td>3623</td>
      <td>4005</td>
      <td>5393</td>
      <td>4752</td>
      <td>4325</td>
      <td>3774</td>
      <td>4331</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>American Samoa</td>
      <td>909</td>
      <td>Oceania</td>
      <td>957</td>
      <td>Polynesia</td>
      <td>902</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>Andorra</td>
      <td>908</td>
      <td>Europe</td>
      <td>925</td>
      <td>Southern Europe</td>
      <td>901</td>
      <td>Developed regions</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 43 columns</p>
</div>



We can also veiw the bottom 5 rows of the dataset using the `tail()` function.


```python
df_can.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Coverage</th>
      <th>OdName</th>
      <th>AREA</th>
      <th>AreaName</th>
      <th>REG</th>
      <th>RegName</th>
      <th>DEV</th>
      <th>DevName</th>
      <th>1980</th>
      <th>...</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>190</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>Viet Nam</td>
      <td>935</td>
      <td>Asia</td>
      <td>920</td>
      <td>South-Eastern Asia</td>
      <td>902</td>
      <td>Developing regions</td>
      <td>1191</td>
      <td>...</td>
      <td>1816</td>
      <td>1852</td>
      <td>3153</td>
      <td>2574</td>
      <td>1784</td>
      <td>2171</td>
      <td>1942</td>
      <td>1723</td>
      <td>1731</td>
      <td>2112</td>
    </tr>
    <tr>
      <th>191</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>Western Sahara</td>
      <td>903</td>
      <td>Africa</td>
      <td>912</td>
      <td>Northern Africa</td>
      <td>902</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>Yemen</td>
      <td>935</td>
      <td>Asia</td>
      <td>922</td>
      <td>Western Asia</td>
      <td>902</td>
      <td>Developing regions</td>
      <td>1</td>
      <td>...</td>
      <td>124</td>
      <td>161</td>
      <td>140</td>
      <td>122</td>
      <td>133</td>
      <td>128</td>
      <td>211</td>
      <td>160</td>
      <td>174</td>
      <td>217</td>
    </tr>
    <tr>
      <th>193</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>Zambia</td>
      <td>903</td>
      <td>Africa</td>
      <td>910</td>
      <td>Eastern Africa</td>
      <td>902</td>
      <td>Developing regions</td>
      <td>11</td>
      <td>...</td>
      <td>56</td>
      <td>91</td>
      <td>77</td>
      <td>71</td>
      <td>64</td>
      <td>60</td>
      <td>102</td>
      <td>69</td>
      <td>46</td>
      <td>59</td>
    </tr>
    <tr>
      <th>194</th>
      <td>Immigrants</td>
      <td>Foreigners</td>
      <td>Zimbabwe</td>
      <td>903</td>
      <td>Africa</td>
      <td>910</td>
      <td>Eastern Africa</td>
      <td>902</td>
      <td>Developing regions</td>
      <td>72</td>
      <td>...</td>
      <td>1450</td>
      <td>615</td>
      <td>454</td>
      <td>663</td>
      <td>611</td>
      <td>508</td>
      <td>494</td>
      <td>434</td>
      <td>437</td>
      <td>407</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 43 columns</p>
</div>



When analyzing a dataset, it's always a good idea to start by getting basic information about your dataframe. We can do this by using the `info()` method.


```python
df_can.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 195 entries, 0 to 194
    Data columns (total 43 columns):
    Type        195 non-null object
    Coverage    195 non-null object
    OdName      195 non-null object
    AREA        195 non-null int64
    AreaName    195 non-null object
    REG         195 non-null int64
    RegName     195 non-null object
    DEV         195 non-null int64
    DevName     195 non-null object
    1980        195 non-null int64
    1981        195 non-null int64
    1982        195 non-null int64
    1983        195 non-null int64
    1984        195 non-null int64
    1985        195 non-null int64
    1986        195 non-null int64
    1987        195 non-null int64
    1988        195 non-null int64
    1989        195 non-null int64
    1990        195 non-null int64
    1991        195 non-null int64
    1992        195 non-null int64
    1993        195 non-null int64
    1994        195 non-null int64
    1995        195 non-null int64
    1996        195 non-null int64
    1997        195 non-null int64
    1998        195 non-null int64
    1999        195 non-null int64
    2000        195 non-null int64
    2001        195 non-null int64
    2002        195 non-null int64
    2003        195 non-null int64
    2004        195 non-null int64
    2005        195 non-null int64
    2006        195 non-null int64
    2007        195 non-null int64
    2008        195 non-null int64
    2009        195 non-null int64
    2010        195 non-null int64
    2011        195 non-null int64
    2012        195 non-null int64
    2013        195 non-null int64
    dtypes: int64(37), object(6)
    memory usage: 65.6+ KB


To get the list of column headers we can call upon the dataframe's `.columns` parameter.


```python
df_can.columns.values 
```




    array(['Type', 'Coverage', 'OdName', 'AREA', 'AreaName', 'REG', 'RegName',
           'DEV', 'DevName', 1980, 1981, 1982, 1983, 1984, 1985, 1986, 1987,
           1988, 1989, 1990, 1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998,
           1999, 2000, 2001, 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009,
           2010, 2011, 2012, 2013], dtype=object)



Similarly, to get the list of indicies we use the `.index` parameter.


```python
df_can.index.values
```




    array([  0,   1,   2,   3,   4,   5,   6,   7,   8,   9,  10,  11,  12,
            13,  14,  15,  16,  17,  18,  19,  20,  21,  22,  23,  24,  25,
            26,  27,  28,  29,  30,  31,  32,  33,  34,  35,  36,  37,  38,
            39,  40,  41,  42,  43,  44,  45,  46,  47,  48,  49,  50,  51,
            52,  53,  54,  55,  56,  57,  58,  59,  60,  61,  62,  63,  64,
            65,  66,  67,  68,  69,  70,  71,  72,  73,  74,  75,  76,  77,
            78,  79,  80,  81,  82,  83,  84,  85,  86,  87,  88,  89,  90,
            91,  92,  93,  94,  95,  96,  97,  98,  99, 100, 101, 102, 103,
           104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116,
           117, 118, 119, 120, 121, 122, 123, 124, 125, 126, 127, 128, 129,
           130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142,
           143, 144, 145, 146, 147, 148, 149, 150, 151, 152, 153, 154, 155,
           156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168,
           169, 170, 171, 172, 173, 174, 175, 176, 177, 178, 179, 180, 181,
           182, 183, 184, 185, 186, 187, 188, 189, 190, 191, 192, 193, 194])



Note: The default type of index and columns is NOT list.


```python
print(type(df_can.columns))
print(type(df_can.index))
```

    <class 'pandas.core.indexes.base.Index'>
    <class 'pandas.core.indexes.range.RangeIndex'>


To get the index and columns as lists, we can use the `tolist()` method.


```python
df_can.columns.tolist()
df_can.index.tolist()

print (type(df_can.columns.tolist()))
print (type(df_can.index.tolist()))
```

    <class 'list'>
    <class 'list'>


To view the dimensions of the dataframe, we use the `.shape` parameter.


```python
# size of dataframe (rows, columns)
df_can.shape    
```




    (195, 43)



Note: The main types stored in *pandas* objects are *float*, *int*, *bool*, *datetime64[ns]* and *datetime64[ns, tz] (in >= 0.17.0)*, *timedelta[ns]*, *category (in >= 0.15.0)*, and *object* (string). In addition these dtypes have item sizes, e.g. int64 and int32. 

Let's clean the data set to remove a few unnecessary columns. We can use *pandas* `drop()` method as follows:


```python
# in pandas axis=0 represents rows (default) and axis=1 represents columns.
df_can.drop(['AREA','REG','DEV','Type','Coverage'], axis=1, inplace=True)
df_can.head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>OdName</th>
      <th>AreaName</th>
      <th>RegName</th>
      <th>DevName</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
      <th>...</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>16</td>
      <td>39</td>
      <td>39</td>
      <td>47</td>
      <td>71</td>
      <td>340</td>
      <td>...</td>
      <td>2978</td>
      <td>3436</td>
      <td>3009</td>
      <td>2652</td>
      <td>2111</td>
      <td>1746</td>
      <td>1758</td>
      <td>2203</td>
      <td>2635</td>
      <td>2004</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>Europe</td>
      <td>Southern Europe</td>
      <td>Developed regions</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>1450</td>
      <td>1223</td>
      <td>856</td>
      <td>702</td>
      <td>560</td>
      <td>716</td>
      <td>561</td>
      <td>539</td>
      <td>620</td>
      <td>603</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 38 columns</p>
</div>



Let's rename the columns so that they make sense. We can use `rename()` method by passing in a dictionary of old and new names as follows:


```python
df_can.rename(columns={'OdName':'Country', 'AreaName':'Continent', 'RegName':'Region'}, inplace=True)
df_can.columns
```




    Index([  'Country', 'Continent',    'Region',   'DevName',        1980,
                  1981,        1982,        1983,        1984,        1985,
                  1986,        1987,        1988,        1989,        1990,
                  1991,        1992,        1993,        1994,        1995,
                  1996,        1997,        1998,        1999,        2000,
                  2001,        2002,        2003,        2004,        2005,
                  2006,        2007,        2008,        2009,        2010,
                  2011,        2012,        2013],
          dtype='object')



We will also add a 'Total' column that sums up the total immigrants by country over the entire period 1980 - 2013, as follows:


```python
df_can['Total'] = df_can.sum(axis=1)
```

We can check to see how many null objects we have in the dataset as follows:


```python
df_can.isnull().sum()
```




    Country      0
    Continent    0
    Region       0
    DevName      0
    1980         0
    1981         0
    1982         0
    1983         0
    1984         0
    1985         0
    1986         0
    1987         0
    1988         0
    1989         0
    1990         0
    1991         0
    1992         0
    1993         0
    1994         0
    1995         0
    1996         0
    1997         0
    1998         0
    1999         0
    2000         0
    2001         0
    2002         0
    2003         0
    2004         0
    2005         0
    2006         0
    2007         0
    2008         0
    2009         0
    2010         0
    2011         0
    2012         0
    2013         0
    Total        0
    dtype: int64



Finally, let's view a quick summary of each column in our dataframe using the `describe()` method.


```python
df_can.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
      <th>1986</th>
      <th>1987</th>
      <th>1988</th>
      <th>1989</th>
      <th>...</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>...</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
      <td>195.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>508.394872</td>
      <td>566.989744</td>
      <td>534.723077</td>
      <td>387.435897</td>
      <td>376.497436</td>
      <td>358.861538</td>
      <td>441.271795</td>
      <td>691.133333</td>
      <td>714.389744</td>
      <td>843.241026</td>
      <td>...</td>
      <td>1320.292308</td>
      <td>1266.958974</td>
      <td>1191.820513</td>
      <td>1246.394872</td>
      <td>1275.733333</td>
      <td>1420.287179</td>
      <td>1262.533333</td>
      <td>1313.958974</td>
      <td>1320.702564</td>
      <td>32867.451282</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1949.588546</td>
      <td>2152.643752</td>
      <td>1866.997511</td>
      <td>1204.333597</td>
      <td>1198.246371</td>
      <td>1079.309600</td>
      <td>1225.576630</td>
      <td>2109.205607</td>
      <td>2443.606788</td>
      <td>2555.048874</td>
      <td>...</td>
      <td>4425.957828</td>
      <td>3926.717747</td>
      <td>3443.542409</td>
      <td>3694.573544</td>
      <td>3829.630424</td>
      <td>4462.946328</td>
      <td>4030.084313</td>
      <td>4247.555161</td>
      <td>4237.951988</td>
      <td>91785.498686</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.500000</td>
      <td>0.500000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>...</td>
      <td>28.500000</td>
      <td>25.000000</td>
      <td>31.000000</td>
      <td>31.000000</td>
      <td>36.000000</td>
      <td>40.500000</td>
      <td>37.500000</td>
      <td>42.500000</td>
      <td>45.000000</td>
      <td>952.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>13.000000</td>
      <td>10.000000</td>
      <td>11.000000</td>
      <td>12.000000</td>
      <td>13.000000</td>
      <td>17.000000</td>
      <td>18.000000</td>
      <td>26.000000</td>
      <td>34.000000</td>
      <td>44.000000</td>
      <td>...</td>
      <td>210.000000</td>
      <td>218.000000</td>
      <td>198.000000</td>
      <td>205.000000</td>
      <td>214.000000</td>
      <td>211.000000</td>
      <td>179.000000</td>
      <td>233.000000</td>
      <td>213.000000</td>
      <td>5018.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>251.500000</td>
      <td>295.500000</td>
      <td>275.000000</td>
      <td>173.000000</td>
      <td>181.000000</td>
      <td>197.000000</td>
      <td>254.000000</td>
      <td>434.000000</td>
      <td>409.000000</td>
      <td>508.500000</td>
      <td>...</td>
      <td>832.000000</td>
      <td>842.000000</td>
      <td>899.000000</td>
      <td>934.500000</td>
      <td>888.000000</td>
      <td>932.000000</td>
      <td>772.000000</td>
      <td>783.000000</td>
      <td>796.000000</td>
      <td>22239.500000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>22045.000000</td>
      <td>24796.000000</td>
      <td>20620.000000</td>
      <td>10015.000000</td>
      <td>10170.000000</td>
      <td>9564.000000</td>
      <td>9470.000000</td>
      <td>21337.000000</td>
      <td>27359.000000</td>
      <td>23795.000000</td>
      <td>...</td>
      <td>42584.000000</td>
      <td>33848.000000</td>
      <td>28742.000000</td>
      <td>30037.000000</td>
      <td>29622.000000</td>
      <td>38617.000000</td>
      <td>36765.000000</td>
      <td>34315.000000</td>
      <td>34129.000000</td>
      <td>691904.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 35 columns</p>
</div>



---
## *pandas* Intermediate: Indexing and Selection (slicing)<a id="6"></a>


### Select Column
**There are two ways to filter on a column name:**

Method 1: Quick and easy, but only works if the column name does NOT have spaces or special characters.
```python
    df.column_name 
        (returns series)
```

Method 2: More robust, and can filter on multiple columns.

```python
    df['column']  
        (returns series)
```

```python 
    df[['column 1', 'column 2']] 
        (returns dataframe)
```
---

Example: Let's try filtering on the list of countries ('Country').


```python
df_can.Country  # returns a series
```




    0         Afghanistan
    1             Albania
    2             Algeria
    3      American Samoa
    4             Andorra
                ...      
    190          Viet Nam
    191    Western Sahara
    192             Yemen
    193            Zambia
    194          Zimbabwe
    Name: Country, Length: 195, dtype: object



Let's try filtering on the list of countries ('OdName') and the data for years: 1980 - 1985.


```python
df_can[['Country', 1980, 1981, 1982, 1983, 1984, 1985]] # returns a dataframe
# notice that 'Country' is string, and the years are integers. 
# for the sake of consistency, we will convert all column names to string later on.
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>16</td>
      <td>39</td>
      <td>39</td>
      <td>47</td>
      <td>71</td>
      <td>340</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>80</td>
      <td>67</td>
      <td>71</td>
      <td>69</td>
      <td>63</td>
      <td>44</td>
    </tr>
    <tr>
      <th>3</th>
      <td>American Samoa</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>190</th>
      <td>Viet Nam</td>
      <td>1191</td>
      <td>1829</td>
      <td>2162</td>
      <td>3404</td>
      <td>7583</td>
      <td>5907</td>
    </tr>
    <tr>
      <th>191</th>
      <td>Western Sahara</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Yemen</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>6</td>
      <td>0</td>
      <td>18</td>
    </tr>
    <tr>
      <th>193</th>
      <td>Zambia</td>
      <td>11</td>
      <td>17</td>
      <td>11</td>
      <td>7</td>
      <td>16</td>
      <td>9</td>
    </tr>
    <tr>
      <th>194</th>
      <td>Zimbabwe</td>
      <td>72</td>
      <td>114</td>
      <td>102</td>
      <td>44</td>
      <td>32</td>
      <td>29</td>
    </tr>
  </tbody>
</table>
<p>195 rows × 7 columns</p>
</div>



### Select Row

There are main 3 ways to select rows:

```python
    df.loc[label]        
        #filters by the labels of the index/column
    df.iloc[index]       
        #filters by the positions of the index/column
```

Before we proceed, notice that the defaul index of the dataset is a numeric range from 0 to 194. This makes it very difficult to do a query by a specific country. For example to search for data on Japan, we need to know the corressponding index value.

This can be fixed very easily by setting the 'Country' column as the index using `set_index()` method.


```python
df_can.set_index('Country', inplace=True)
# tip: The opposite of set is reset. So to reset the index, we can use df_can.reset_index()
```


```python
df_can.head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Continent</th>
      <th>Region</th>
      <th>DevName</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
      <th>1986</th>
      <th>...</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>Total</th>
    </tr>
    <tr>
      <th>Country</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Afghanistan</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>16</td>
      <td>39</td>
      <td>39</td>
      <td>47</td>
      <td>71</td>
      <td>340</td>
      <td>496</td>
      <td>...</td>
      <td>3436</td>
      <td>3009</td>
      <td>2652</td>
      <td>2111</td>
      <td>1746</td>
      <td>1758</td>
      <td>2203</td>
      <td>2635</td>
      <td>2004</td>
      <td>58639</td>
    </tr>
    <tr>
      <th>Albania</th>
      <td>Europe</td>
      <td>Southern Europe</td>
      <td>Developed regions</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>1223</td>
      <td>856</td>
      <td>702</td>
      <td>560</td>
      <td>716</td>
      <td>561</td>
      <td>539</td>
      <td>620</td>
      <td>603</td>
      <td>15699</td>
    </tr>
    <tr>
      <th>Algeria</th>
      <td>Africa</td>
      <td>Northern Africa</td>
      <td>Developing regions</td>
      <td>80</td>
      <td>67</td>
      <td>71</td>
      <td>69</td>
      <td>63</td>
      <td>44</td>
      <td>69</td>
      <td>...</td>
      <td>3626</td>
      <td>4807</td>
      <td>3623</td>
      <td>4005</td>
      <td>5393</td>
      <td>4752</td>
      <td>4325</td>
      <td>3774</td>
      <td>4331</td>
      <td>69439</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 38 columns</p>
</div>




```python
# optional: to remove the name of the index
df_can.index.name = None
```

Example: Let's view the number of immigrants from Japan (row 87) for the following scenarios:
    1. The full row data (all columns)
    2. For year 2013
    3. For years 1980 to 1985


```python
# 1. the full row data (all columns)
print(df_can.loc['Japan'])

# alternate methods
print(df_can.iloc[87])
print(df_can[df_can.index == 'Japan'].T.squeeze())
```

    Continent                 Asia
    Region            Eastern Asia
    DevName      Developed regions
    1980                       701
    1981                       756
    1982                       598
    1983                       309
    1984                       246
    1985                       198
    1986                       248
    1987                       422
    1988                       324
    1989                       494
    1990                       379
    1991                       506
    1992                       605
    1993                       907
    1994                       956
    1995                       826
    1996                       994
    1997                       924
    1998                       897
    1999                      1083
    2000                      1010
    2001                      1092
    2002                       806
    2003                       817
    2004                       973
    2005                      1067
    2006                      1212
    2007                      1250
    2008                      1284
    2009                      1194
    2010                      1168
    2011                      1265
    2012                      1214
    2013                       982
    Total                    27707
    Name: Japan, dtype: object
    Continent                 Asia
    Region            Eastern Asia
    DevName      Developed regions
    1980                       701
    1981                       756
    1982                       598
    1983                       309
    1984                       246
    1985                       198
    1986                       248
    1987                       422
    1988                       324
    1989                       494
    1990                       379
    1991                       506
    1992                       605
    1993                       907
    1994                       956
    1995                       826
    1996                       994
    1997                       924
    1998                       897
    1999                      1083
    2000                      1010
    2001                      1092
    2002                       806
    2003                       817
    2004                       973
    2005                      1067
    2006                      1212
    2007                      1250
    2008                      1284
    2009                      1194
    2010                      1168
    2011                      1265
    2012                      1214
    2013                       982
    Total                    27707
    Name: Japan, dtype: object
    Continent                 Asia
    Region            Eastern Asia
    DevName      Developed regions
    1980                       701
    1981                       756
    1982                       598
    1983                       309
    1984                       246
    1985                       198
    1986                       248
    1987                       422
    1988                       324
    1989                       494
    1990                       379
    1991                       506
    1992                       605
    1993                       907
    1994                       956
    1995                       826
    1996                       994
    1997                       924
    1998                       897
    1999                      1083
    2000                      1010
    2001                      1092
    2002                       806
    2003                       817
    2004                       973
    2005                      1067
    2006                      1212
    2007                      1250
    2008                      1284
    2009                      1194
    2010                      1168
    2011                      1265
    2012                      1214
    2013                       982
    Total                    27707
    Name: Japan, dtype: object



```python
# 2. for year 2013
print(df_can.loc['Japan', 2013])

# alternate method
print(df_can.iloc[87, 36]) # year 2013 is the last column, with a positional index of 36
```

    982
    982



```python
# 3. for years 1980 to 1985
print(df_can.loc['Japan', [1980, 1981, 1982, 1983, 1984, 1984]])
print(df_can.iloc[87, [3, 4, 5, 6, 7, 8]])
```

    1980    701
    1981    756
    1982    598
    1983    309
    1984    246
    1984    246
    Name: Japan, dtype: object
    1980    701
    1981    756
    1982    598
    1983    309
    1984    246
    1985    198
    Name: Japan, dtype: object


Column names that are integers (such as the years) might introduce some confusion. For example, when we are referencing the year 2013, one might confuse that when the 2013th positional index. 

To avoid this ambuigity, let's convert the column names into strings: '1980' to '2013'.


```python
df_can.columns = list(map(str, df_can.columns))
# [print (type(x)) for x in df_can.columns.values] #<-- uncomment to check type of column headers
```

Since we converted the years to string, let's declare a variable that will allow us to easily call upon the full range of years:


```python
# useful for plotting later on
years = list(map(str, range(1980, 2014)))
years
```




    ['1980',
     '1981',
     '1982',
     '1983',
     '1984',
     '1985',
     '1986',
     '1987',
     '1988',
     '1989',
     '1990',
     '1991',
     '1992',
     '1993',
     '1994',
     '1995',
     '1996',
     '1997',
     '1998',
     '1999',
     '2000',
     '2001',
     '2002',
     '2003',
     '2004',
     '2005',
     '2006',
     '2007',
     '2008',
     '2009',
     '2010',
     '2011',
     '2012',
     '2013']



### Filtering based on a criteria
To filter the dataframe based on a condition, we simply pass the condition as a boolean vector. 

For example, Let's filter the dataframe to show the data on Asian countries (AreaName = Asia).


```python
# 1. create the condition boolean series
condition = df_can['Continent'] == 'Asia'
print(condition)
```

    Afghanistan        True
    Albania           False
    Algeria           False
    American Samoa    False
    Andorra           False
                      ...  
    Viet Nam           True
    Western Sahara    False
    Yemen              True
    Zambia            False
    Zimbabwe          False
    Name: Continent, Length: 195, dtype: bool



```python
# 2. pass this condition into the dataFrame
df_can[condition]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Continent</th>
      <th>Region</th>
      <th>DevName</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
      <th>1986</th>
      <th>...</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Afghanistan</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>16</td>
      <td>39</td>
      <td>39</td>
      <td>47</td>
      <td>71</td>
      <td>340</td>
      <td>496</td>
      <td>...</td>
      <td>3436</td>
      <td>3009</td>
      <td>2652</td>
      <td>2111</td>
      <td>1746</td>
      <td>1758</td>
      <td>2203</td>
      <td>2635</td>
      <td>2004</td>
      <td>58639</td>
    </tr>
    <tr>
      <th>Armenia</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>224</td>
      <td>218</td>
      <td>198</td>
      <td>205</td>
      <td>267</td>
      <td>252</td>
      <td>236</td>
      <td>258</td>
      <td>207</td>
      <td>3310</td>
    </tr>
    <tr>
      <th>Azerbaijan</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>359</td>
      <td>236</td>
      <td>203</td>
      <td>125</td>
      <td>165</td>
      <td>209</td>
      <td>138</td>
      <td>161</td>
      <td>57</td>
      <td>2649</td>
    </tr>
    <tr>
      <th>Bahrain</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>...</td>
      <td>12</td>
      <td>12</td>
      <td>22</td>
      <td>9</td>
      <td>35</td>
      <td>28</td>
      <td>21</td>
      <td>39</td>
      <td>32</td>
      <td>475</td>
    </tr>
    <tr>
      <th>Bangladesh</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>83</td>
      <td>84</td>
      <td>86</td>
      <td>81</td>
      <td>98</td>
      <td>92</td>
      <td>486</td>
      <td>...</td>
      <td>4171</td>
      <td>4014</td>
      <td>2897</td>
      <td>2939</td>
      <td>2104</td>
      <td>4721</td>
      <td>2694</td>
      <td>2640</td>
      <td>3789</td>
      <td>65568</td>
    </tr>
    <tr>
      <th>Bhutan</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>5</td>
      <td>10</td>
      <td>7</td>
      <td>36</td>
      <td>865</td>
      <td>1464</td>
      <td>1879</td>
      <td>1075</td>
      <td>487</td>
      <td>5876</td>
    </tr>
    <tr>
      <th>Brunei Darussalam</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>79</td>
      <td>6</td>
      <td>8</td>
      <td>2</td>
      <td>2</td>
      <td>4</td>
      <td>12</td>
      <td>...</td>
      <td>4</td>
      <td>5</td>
      <td>11</td>
      <td>10</td>
      <td>5</td>
      <td>12</td>
      <td>6</td>
      <td>3</td>
      <td>6</td>
      <td>600</td>
    </tr>
    <tr>
      <th>Cambodia</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>12</td>
      <td>19</td>
      <td>26</td>
      <td>33</td>
      <td>10</td>
      <td>7</td>
      <td>8</td>
      <td>...</td>
      <td>370</td>
      <td>529</td>
      <td>460</td>
      <td>354</td>
      <td>203</td>
      <td>200</td>
      <td>196</td>
      <td>233</td>
      <td>288</td>
      <td>6538</td>
    </tr>
    <tr>
      <th>China</th>
      <td>Asia</td>
      <td>Eastern Asia</td>
      <td>Developing regions</td>
      <td>5123</td>
      <td>6682</td>
      <td>3308</td>
      <td>1863</td>
      <td>1527</td>
      <td>1816</td>
      <td>1960</td>
      <td>...</td>
      <td>42584</td>
      <td>33518</td>
      <td>27642</td>
      <td>30037</td>
      <td>29622</td>
      <td>30391</td>
      <td>28502</td>
      <td>33024</td>
      <td>34129</td>
      <td>659962</td>
    </tr>
    <tr>
      <th>China, Hong Kong Special Administrative Region</th>
      <td>Asia</td>
      <td>Eastern Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>729</td>
      <td>712</td>
      <td>674</td>
      <td>897</td>
      <td>657</td>
      <td>623</td>
      <td>591</td>
      <td>728</td>
      <td>774</td>
      <td>9327</td>
    </tr>
    <tr>
      <th>China, Macao Special Administrative Region</th>
      <td>Asia</td>
      <td>Eastern Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>21</td>
      <td>32</td>
      <td>16</td>
      <td>12</td>
      <td>21</td>
      <td>21</td>
      <td>13</td>
      <td>33</td>
      <td>29</td>
      <td>284</td>
    </tr>
    <tr>
      <th>Cyprus</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>132</td>
      <td>128</td>
      <td>84</td>
      <td>46</td>
      <td>46</td>
      <td>43</td>
      <td>48</td>
      <td>...</td>
      <td>7</td>
      <td>9</td>
      <td>4</td>
      <td>7</td>
      <td>6</td>
      <td>18</td>
      <td>6</td>
      <td>12</td>
      <td>16</td>
      <td>1126</td>
    </tr>
    <tr>
      <th>Democratic People's Republic of Korea</th>
      <td>Asia</td>
      <td>Eastern Asia</td>
      <td>Developing regions</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>...</td>
      <td>14</td>
      <td>10</td>
      <td>7</td>
      <td>19</td>
      <td>11</td>
      <td>45</td>
      <td>97</td>
      <td>66</td>
      <td>17</td>
      <td>388</td>
    </tr>
    <tr>
      <th>Georgia</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>114</td>
      <td>125</td>
      <td>132</td>
      <td>112</td>
      <td>128</td>
      <td>126</td>
      <td>139</td>
      <td>147</td>
      <td>125</td>
      <td>2068</td>
    </tr>
    <tr>
      <th>India</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>8880</td>
      <td>8670</td>
      <td>8147</td>
      <td>7338</td>
      <td>5704</td>
      <td>4211</td>
      <td>7150</td>
      <td>...</td>
      <td>36210</td>
      <td>33848</td>
      <td>28742</td>
      <td>28261</td>
      <td>29456</td>
      <td>34235</td>
      <td>27509</td>
      <td>30933</td>
      <td>33087</td>
      <td>691904</td>
    </tr>
    <tr>
      <th>Indonesia</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>186</td>
      <td>178</td>
      <td>252</td>
      <td>115</td>
      <td>123</td>
      <td>100</td>
      <td>127</td>
      <td>...</td>
      <td>632</td>
      <td>613</td>
      <td>657</td>
      <td>661</td>
      <td>504</td>
      <td>712</td>
      <td>390</td>
      <td>395</td>
      <td>387</td>
      <td>13150</td>
    </tr>
    <tr>
      <th>Iran (Islamic Republic of)</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>1172</td>
      <td>1429</td>
      <td>1822</td>
      <td>1592</td>
      <td>1977</td>
      <td>1648</td>
      <td>1794</td>
      <td>...</td>
      <td>5837</td>
      <td>7480</td>
      <td>6974</td>
      <td>6475</td>
      <td>6580</td>
      <td>7477</td>
      <td>7479</td>
      <td>7534</td>
      <td>11291</td>
      <td>175923</td>
    </tr>
    <tr>
      <th>Iraq</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>262</td>
      <td>245</td>
      <td>260</td>
      <td>380</td>
      <td>428</td>
      <td>231</td>
      <td>265</td>
      <td>...</td>
      <td>2226</td>
      <td>1788</td>
      <td>2406</td>
      <td>3543</td>
      <td>5450</td>
      <td>5941</td>
      <td>6196</td>
      <td>4041</td>
      <td>4918</td>
      <td>69789</td>
    </tr>
    <tr>
      <th>Israel</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>1403</td>
      <td>1711</td>
      <td>1334</td>
      <td>541</td>
      <td>446</td>
      <td>680</td>
      <td>1212</td>
      <td>...</td>
      <td>2446</td>
      <td>2625</td>
      <td>2401</td>
      <td>2562</td>
      <td>2316</td>
      <td>2755</td>
      <td>1970</td>
      <td>2134</td>
      <td>1945</td>
      <td>66508</td>
    </tr>
    <tr>
      <th>Japan</th>
      <td>Asia</td>
      <td>Eastern Asia</td>
      <td>Developed regions</td>
      <td>701</td>
      <td>756</td>
      <td>598</td>
      <td>309</td>
      <td>246</td>
      <td>198</td>
      <td>248</td>
      <td>...</td>
      <td>1067</td>
      <td>1212</td>
      <td>1250</td>
      <td>1284</td>
      <td>1194</td>
      <td>1168</td>
      <td>1265</td>
      <td>1214</td>
      <td>982</td>
      <td>27707</td>
    </tr>
    <tr>
      <th>Jordan</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>177</td>
      <td>160</td>
      <td>155</td>
      <td>113</td>
      <td>102</td>
      <td>179</td>
      <td>181</td>
      <td>...</td>
      <td>1940</td>
      <td>1827</td>
      <td>1421</td>
      <td>1581</td>
      <td>1235</td>
      <td>1831</td>
      <td>1635</td>
      <td>1206</td>
      <td>1255</td>
      <td>35406</td>
    </tr>
    <tr>
      <th>Kazakhstan</th>
      <td>Asia</td>
      <td>Central Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>506</td>
      <td>408</td>
      <td>436</td>
      <td>394</td>
      <td>431</td>
      <td>377</td>
      <td>381</td>
      <td>462</td>
      <td>348</td>
      <td>8490</td>
    </tr>
    <tr>
      <th>Kuwait</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>1</td>
      <td>0</td>
      <td>8</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>4</td>
      <td>...</td>
      <td>66</td>
      <td>35</td>
      <td>62</td>
      <td>53</td>
      <td>68</td>
      <td>67</td>
      <td>58</td>
      <td>73</td>
      <td>48</td>
      <td>2025</td>
    </tr>
    <tr>
      <th>Kyrgyzstan</th>
      <td>Asia</td>
      <td>Central Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>173</td>
      <td>161</td>
      <td>135</td>
      <td>168</td>
      <td>173</td>
      <td>157</td>
      <td>159</td>
      <td>278</td>
      <td>123</td>
      <td>2353</td>
    </tr>
    <tr>
      <th>Lao People's Democratic Republic</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>11</td>
      <td>6</td>
      <td>16</td>
      <td>16</td>
      <td>7</td>
      <td>17</td>
      <td>21</td>
      <td>...</td>
      <td>42</td>
      <td>74</td>
      <td>53</td>
      <td>32</td>
      <td>39</td>
      <td>54</td>
      <td>22</td>
      <td>25</td>
      <td>15</td>
      <td>1089</td>
    </tr>
    <tr>
      <th>Lebanon</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>1409</td>
      <td>1119</td>
      <td>1159</td>
      <td>789</td>
      <td>1253</td>
      <td>1683</td>
      <td>2576</td>
      <td>...</td>
      <td>3709</td>
      <td>3802</td>
      <td>3467</td>
      <td>3566</td>
      <td>3077</td>
      <td>3432</td>
      <td>3072</td>
      <td>1614</td>
      <td>2172</td>
      <td>115359</td>
    </tr>
    <tr>
      <th>Malaysia</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>786</td>
      <td>816</td>
      <td>813</td>
      <td>448</td>
      <td>384</td>
      <td>374</td>
      <td>425</td>
      <td>...</td>
      <td>593</td>
      <td>580</td>
      <td>600</td>
      <td>658</td>
      <td>640</td>
      <td>802</td>
      <td>409</td>
      <td>358</td>
      <td>204</td>
      <td>24417</td>
    </tr>
    <tr>
      <th>Maldives</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>7</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>30</td>
    </tr>
    <tr>
      <th>Mongolia</th>
      <td>Asia</td>
      <td>Eastern Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>59</td>
      <td>64</td>
      <td>82</td>
      <td>59</td>
      <td>118</td>
      <td>169</td>
      <td>103</td>
      <td>68</td>
      <td>99</td>
      <td>952</td>
    </tr>
    <tr>
      <th>Myanmar</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>80</td>
      <td>62</td>
      <td>46</td>
      <td>31</td>
      <td>41</td>
      <td>23</td>
      <td>18</td>
      <td>...</td>
      <td>210</td>
      <td>953</td>
      <td>1887</td>
      <td>975</td>
      <td>1153</td>
      <td>556</td>
      <td>368</td>
      <td>193</td>
      <td>262</td>
      <td>9245</td>
    </tr>
    <tr>
      <th>Nepal</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>1</td>
      <td>1</td>
      <td>6</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>13</td>
      <td>...</td>
      <td>607</td>
      <td>540</td>
      <td>511</td>
      <td>581</td>
      <td>561</td>
      <td>1392</td>
      <td>1129</td>
      <td>1185</td>
      <td>1308</td>
      <td>10222</td>
    </tr>
    <tr>
      <th>Oman</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>14</td>
      <td>18</td>
      <td>16</td>
      <td>10</td>
      <td>7</td>
      <td>14</td>
      <td>10</td>
      <td>13</td>
      <td>11</td>
      <td>224</td>
    </tr>
    <tr>
      <th>Pakistan</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>978</td>
      <td>972</td>
      <td>1201</td>
      <td>900</td>
      <td>668</td>
      <td>514</td>
      <td>691</td>
      <td>...</td>
      <td>14314</td>
      <td>13127</td>
      <td>10124</td>
      <td>8994</td>
      <td>7217</td>
      <td>6811</td>
      <td>7468</td>
      <td>11227</td>
      <td>12603</td>
      <td>241600</td>
    </tr>
    <tr>
      <th>Philippines</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>6051</td>
      <td>5921</td>
      <td>5249</td>
      <td>4562</td>
      <td>3801</td>
      <td>3150</td>
      <td>4166</td>
      <td>...</td>
      <td>18139</td>
      <td>18400</td>
      <td>19837</td>
      <td>24887</td>
      <td>28573</td>
      <td>38617</td>
      <td>36765</td>
      <td>34315</td>
      <td>29544</td>
      <td>511391</td>
    </tr>
    <tr>
      <th>Qatar</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>11</td>
      <td>2</td>
      <td>5</td>
      <td>9</td>
      <td>6</td>
      <td>18</td>
      <td>3</td>
      <td>14</td>
      <td>6</td>
      <td>157</td>
    </tr>
    <tr>
      <th>Republic of Korea</th>
      <td>Asia</td>
      <td>Eastern Asia</td>
      <td>Developing regions</td>
      <td>1011</td>
      <td>1456</td>
      <td>1572</td>
      <td>1081</td>
      <td>847</td>
      <td>962</td>
      <td>1208</td>
      <td>...</td>
      <td>5832</td>
      <td>6215</td>
      <td>5920</td>
      <td>7294</td>
      <td>5874</td>
      <td>5537</td>
      <td>4588</td>
      <td>5316</td>
      <td>4509</td>
      <td>142581</td>
    </tr>
    <tr>
      <th>Saudi Arabia</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>...</td>
      <td>198</td>
      <td>252</td>
      <td>188</td>
      <td>249</td>
      <td>246</td>
      <td>330</td>
      <td>278</td>
      <td>286</td>
      <td>267</td>
      <td>3425</td>
    </tr>
    <tr>
      <th>Singapore</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>241</td>
      <td>301</td>
      <td>337</td>
      <td>169</td>
      <td>128</td>
      <td>139</td>
      <td>205</td>
      <td>...</td>
      <td>392</td>
      <td>298</td>
      <td>690</td>
      <td>734</td>
      <td>366</td>
      <td>805</td>
      <td>219</td>
      <td>146</td>
      <td>141</td>
      <td>14579</td>
    </tr>
    <tr>
      <th>Sri Lanka</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>185</td>
      <td>371</td>
      <td>290</td>
      <td>197</td>
      <td>1086</td>
      <td>845</td>
      <td>1838</td>
      <td>...</td>
      <td>4930</td>
      <td>4714</td>
      <td>4123</td>
      <td>4756</td>
      <td>4547</td>
      <td>4422</td>
      <td>3309</td>
      <td>3338</td>
      <td>2394</td>
      <td>148358</td>
    </tr>
    <tr>
      <th>State of Palestine</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>453</td>
      <td>627</td>
      <td>441</td>
      <td>481</td>
      <td>400</td>
      <td>654</td>
      <td>555</td>
      <td>533</td>
      <td>462</td>
      <td>6512</td>
    </tr>
    <tr>
      <th>Syrian Arab Republic</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>315</td>
      <td>419</td>
      <td>409</td>
      <td>269</td>
      <td>264</td>
      <td>385</td>
      <td>493</td>
      <td>...</td>
      <td>1458</td>
      <td>1145</td>
      <td>1056</td>
      <td>919</td>
      <td>917</td>
      <td>1039</td>
      <td>1005</td>
      <td>650</td>
      <td>1009</td>
      <td>31485</td>
    </tr>
    <tr>
      <th>Tajikistan</th>
      <td>Asia</td>
      <td>Central Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>85</td>
      <td>46</td>
      <td>44</td>
      <td>15</td>
      <td>50</td>
      <td>52</td>
      <td>47</td>
      <td>34</td>
      <td>39</td>
      <td>503</td>
    </tr>
    <tr>
      <th>Thailand</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>56</td>
      <td>53</td>
      <td>113</td>
      <td>65</td>
      <td>82</td>
      <td>66</td>
      <td>78</td>
      <td>...</td>
      <td>575</td>
      <td>500</td>
      <td>487</td>
      <td>519</td>
      <td>512</td>
      <td>499</td>
      <td>396</td>
      <td>296</td>
      <td>400</td>
      <td>9174</td>
    </tr>
    <tr>
      <th>Turkey</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>481</td>
      <td>874</td>
      <td>706</td>
      <td>280</td>
      <td>338</td>
      <td>202</td>
      <td>257</td>
      <td>...</td>
      <td>2065</td>
      <td>1638</td>
      <td>1463</td>
      <td>1122</td>
      <td>1238</td>
      <td>1492</td>
      <td>1257</td>
      <td>1068</td>
      <td>729</td>
      <td>31781</td>
    </tr>
    <tr>
      <th>Turkmenistan</th>
      <td>Asia</td>
      <td>Central Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>40</td>
      <td>26</td>
      <td>37</td>
      <td>13</td>
      <td>20</td>
      <td>30</td>
      <td>20</td>
      <td>20</td>
      <td>14</td>
      <td>310</td>
    </tr>
    <tr>
      <th>United Arab Emirates</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>5</td>
      <td>...</td>
      <td>31</td>
      <td>42</td>
      <td>37</td>
      <td>33</td>
      <td>37</td>
      <td>86</td>
      <td>60</td>
      <td>54</td>
      <td>46</td>
      <td>836</td>
    </tr>
    <tr>
      <th>Uzbekistan</th>
      <td>Asia</td>
      <td>Central Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>330</td>
      <td>262</td>
      <td>284</td>
      <td>215</td>
      <td>288</td>
      <td>289</td>
      <td>162</td>
      <td>235</td>
      <td>167</td>
      <td>3368</td>
    </tr>
    <tr>
      <th>Viet Nam</th>
      <td>Asia</td>
      <td>South-Eastern Asia</td>
      <td>Developing regions</td>
      <td>1191</td>
      <td>1829</td>
      <td>2162</td>
      <td>3404</td>
      <td>7583</td>
      <td>5907</td>
      <td>2741</td>
      <td>...</td>
      <td>1852</td>
      <td>3153</td>
      <td>2574</td>
      <td>1784</td>
      <td>2171</td>
      <td>1942</td>
      <td>1723</td>
      <td>1731</td>
      <td>2112</td>
      <td>97146</td>
    </tr>
    <tr>
      <th>Yemen</th>
      <td>Asia</td>
      <td>Western Asia</td>
      <td>Developing regions</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>6</td>
      <td>0</td>
      <td>18</td>
      <td>7</td>
      <td>...</td>
      <td>161</td>
      <td>140</td>
      <td>122</td>
      <td>133</td>
      <td>128</td>
      <td>211</td>
      <td>160</td>
      <td>174</td>
      <td>217</td>
      <td>2985</td>
    </tr>
  </tbody>
</table>
<p>49 rows × 38 columns</p>
</div>




```python
# we can pass mutliple criteria in the same line. 
# let's filter for AreaNAme = Asia and RegName = Southern Asia

df_can[(df_can['Continent']=='Asia') & (df_can['Region']=='Southern Asia')]

# note: When using 'and' and 'or' operators, pandas requires we use '&' and '|' instead of 'and' and 'or'
# don't forget to enclose the two conditions in parentheses
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Continent</th>
      <th>Region</th>
      <th>DevName</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
      <th>1986</th>
      <th>...</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Afghanistan</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>16</td>
      <td>39</td>
      <td>39</td>
      <td>47</td>
      <td>71</td>
      <td>340</td>
      <td>496</td>
      <td>...</td>
      <td>3436</td>
      <td>3009</td>
      <td>2652</td>
      <td>2111</td>
      <td>1746</td>
      <td>1758</td>
      <td>2203</td>
      <td>2635</td>
      <td>2004</td>
      <td>58639</td>
    </tr>
    <tr>
      <th>Bangladesh</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>83</td>
      <td>84</td>
      <td>86</td>
      <td>81</td>
      <td>98</td>
      <td>92</td>
      <td>486</td>
      <td>...</td>
      <td>4171</td>
      <td>4014</td>
      <td>2897</td>
      <td>2939</td>
      <td>2104</td>
      <td>4721</td>
      <td>2694</td>
      <td>2640</td>
      <td>3789</td>
      <td>65568</td>
    </tr>
    <tr>
      <th>Bhutan</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>5</td>
      <td>10</td>
      <td>7</td>
      <td>36</td>
      <td>865</td>
      <td>1464</td>
      <td>1879</td>
      <td>1075</td>
      <td>487</td>
      <td>5876</td>
    </tr>
    <tr>
      <th>India</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>8880</td>
      <td>8670</td>
      <td>8147</td>
      <td>7338</td>
      <td>5704</td>
      <td>4211</td>
      <td>7150</td>
      <td>...</td>
      <td>36210</td>
      <td>33848</td>
      <td>28742</td>
      <td>28261</td>
      <td>29456</td>
      <td>34235</td>
      <td>27509</td>
      <td>30933</td>
      <td>33087</td>
      <td>691904</td>
    </tr>
    <tr>
      <th>Iran (Islamic Republic of)</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>1172</td>
      <td>1429</td>
      <td>1822</td>
      <td>1592</td>
      <td>1977</td>
      <td>1648</td>
      <td>1794</td>
      <td>...</td>
      <td>5837</td>
      <td>7480</td>
      <td>6974</td>
      <td>6475</td>
      <td>6580</td>
      <td>7477</td>
      <td>7479</td>
      <td>7534</td>
      <td>11291</td>
      <td>175923</td>
    </tr>
    <tr>
      <th>Maldives</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>7</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>30</td>
    </tr>
    <tr>
      <th>Nepal</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>1</td>
      <td>1</td>
      <td>6</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>13</td>
      <td>...</td>
      <td>607</td>
      <td>540</td>
      <td>511</td>
      <td>581</td>
      <td>561</td>
      <td>1392</td>
      <td>1129</td>
      <td>1185</td>
      <td>1308</td>
      <td>10222</td>
    </tr>
    <tr>
      <th>Pakistan</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>978</td>
      <td>972</td>
      <td>1201</td>
      <td>900</td>
      <td>668</td>
      <td>514</td>
      <td>691</td>
      <td>...</td>
      <td>14314</td>
      <td>13127</td>
      <td>10124</td>
      <td>8994</td>
      <td>7217</td>
      <td>6811</td>
      <td>7468</td>
      <td>11227</td>
      <td>12603</td>
      <td>241600</td>
    </tr>
    <tr>
      <th>Sri Lanka</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>185</td>
      <td>371</td>
      <td>290</td>
      <td>197</td>
      <td>1086</td>
      <td>845</td>
      <td>1838</td>
      <td>...</td>
      <td>4930</td>
      <td>4714</td>
      <td>4123</td>
      <td>4756</td>
      <td>4547</td>
      <td>4422</td>
      <td>3309</td>
      <td>3338</td>
      <td>2394</td>
      <td>148358</td>
    </tr>
  </tbody>
</table>
<p>9 rows × 38 columns</p>
</div>



Before we proceed: let's review the changes we have made to our dataframe.


```python
print('data dimensions:', df_can.shape)
print(df_can.columns)
df_can.head(2)
```

    data dimensions: (195, 38)
    Index(['Continent', 'Region', 'DevName', '1980', '1981', '1982', '1983',
           '1984', '1985', '1986', '1987', '1988', '1989', '1990', '1991', '1992',
           '1993', '1994', '1995', '1996', '1997', '1998', '1999', '2000', '2001',
           '2002', '2003', '2004', '2005', '2006', '2007', '2008', '2009', '2010',
           '2011', '2012', '2013', 'Total'],
          dtype='object')





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Continent</th>
      <th>Region</th>
      <th>DevName</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
      <th>1986</th>
      <th>...</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Afghanistan</th>
      <td>Asia</td>
      <td>Southern Asia</td>
      <td>Developing regions</td>
      <td>16</td>
      <td>39</td>
      <td>39</td>
      <td>47</td>
      <td>71</td>
      <td>340</td>
      <td>496</td>
      <td>...</td>
      <td>3436</td>
      <td>3009</td>
      <td>2652</td>
      <td>2111</td>
      <td>1746</td>
      <td>1758</td>
      <td>2203</td>
      <td>2635</td>
      <td>2004</td>
      <td>58639</td>
    </tr>
    <tr>
      <th>Albania</th>
      <td>Europe</td>
      <td>Southern Europe</td>
      <td>Developed regions</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>1223</td>
      <td>856</td>
      <td>702</td>
      <td>560</td>
      <td>716</td>
      <td>561</td>
      <td>539</td>
      <td>620</td>
      <td>603</td>
      <td>15699</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 38 columns</p>
</div>



---
# Visualizing Data using Matplotlib<a id="8"></a>

## Matplotlib: Standard Python Visualization Library<a id="10"></a>

The primary plotting library we will explore in the course is [Matplotlib](http://matplotlib.org/).  As mentioned on their website: 
>Matplotlib is a Python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms. Matplotlib can be used in Python scripts, the Python and IPython shell, the jupyter notebook, web application servers, and four graphical user interface toolkits.

If you are aspiring to create impactful visualization with python, Matplotlib is an essential tool to have at your disposal.

### Matplotlib.Pyplot

One of the core aspects of Matplotlib is `matplotlib.pyplot`. It is Matplotlib's scripting layer which we studied in details in the videos about Matplotlib. Recall that it is a collection of command style functions that make Matplotlib work like MATLAB. Each `pyplot` function makes some change to a figure: e.g., creates a figure, creates a plotting area in a figure, plots some lines in a plotting area, decorates the plot with labels, etc. In this lab, we will work with the scripting layer to learn how to generate line plots. In future labs, we will get to work with the Artist layer as well to experiment first hand how it differs from the scripting layer. 


Let's start by importing `Matplotlib` and `Matplotlib.pyplot` as follows:


```python
# we are using the inline backend
%matplotlib inline 

import matplotlib as mpl
import matplotlib.pyplot as plt
```

*optional: check if Matplotlib is loaded.


```python
print ('Matplotlib version: ', mpl.__version__) # >= 2.0.0
```

    Matplotlib version:  3.1.1


*optional: apply a style to Matplotlib.


```python
print(plt.style.available)
mpl.style.use(['ggplot']) # optional: for ggplot-like style
```

    ['Solarize_Light2', '_classic_test', 'bmh', 'classic', 'dark_background', 'fast', 'fivethirtyeight', 'ggplot', 'grayscale', 'seaborn-bright', 'seaborn-colorblind', 'seaborn-dark-palette', 'seaborn-dark', 'seaborn-darkgrid', 'seaborn-deep', 'seaborn-muted', 'seaborn-notebook', 'seaborn-paper', 'seaborn-pastel', 'seaborn-poster', 'seaborn-talk', 'seaborn-ticks', 'seaborn-white', 'seaborn-whitegrid', 'seaborn', 'tableau-colorblind10']


### Plotting in *pandas*

Fortunately, pandas has a built-in implementation of Matplotlib that we can use. Plotting in *pandas* is as simple as appending a `.plot()` method to a series or dataframe.

Documentation:
- [Plotting with Series](http://pandas.pydata.org/pandas-docs/stable/api.html#plotting)<br>
- [Plotting with Dataframes](http://pandas.pydata.org/pandas-docs/stable/api.html#api-dataframe-plotting)

# Line Pots (Series/Dataframe) <a id="12"></a>

**What is a line plot and why use it?**

A line chart or line plot is a type of plot which displays information as a series of data points called 'markers' connected by straight line segments. It is a basic type of chart common in many fields.
Use line plot when you have a continuous data set. These are best suited for trend-based visualizations of data over a period of time.

**Let's start with a case study:**

In 2010, Haiti suffered a catastrophic magnitude 7.0 earthquake. The quake caused widespread devastation and loss of life and aout three million people were affected by this natural disaster. As part of Canada's humanitarian effort, the Government of Canada stepped up its effort in accepting refugees from Haiti. We can quickly visualize this effort using a `Line` plot:

**Question:** Plot a line graph of immigration from Haiti using `df.plot()`.


First, we will extract the data series for Haiti.


```python
haiti = df_can.loc['Haiti', years] # passing in years 1980 - 2013 to exclude the 'total' column
haiti.head()
```




    1980    1666
    1981    3692
    1982    3498
    1983    2860
    1984    1418
    Name: Haiti, dtype: object



Next, we will plot a line plot by appending `.plot()` to the `haiti` dataframe.


```python
haiti.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f91352e8128>




![png](output_81_1.png)


*pandas* automatically populated the x-axis with the index values (years), and the y-axis with the column values (population). However, notice how the years were not displayed because they are of type *string*. Therefore, let's change the type of the index values to *integer* for plotting.

Also, let's label the x and y axis using `plt.title()`, `plt.ylabel()`, and `plt.xlabel()` as follows:


```python
haiti.index = haiti.index.map(int) # let's change the index values of Haiti to type integer for plotting
haiti.plot(kind='line')

plt.title('Immigration from Haiti')
plt.ylabel('Number of immigrants')
plt.xlabel('Years')

plt.show() # need this line to show the updates made to the figure
```


![png](output_83_0.png)


We can clearly notice how number of immigrants from Haiti spiked up from 2010 as Canada stepped up its efforts to accept refugees from Haiti. Let's annotate this spike in the plot by using the `plt.text()` method.


```python
haiti.plot(kind='line')

plt.title('Immigration from Haiti')
plt.ylabel('Number of Immigrants')
plt.xlabel('Years')

# annotate the 2010 Earthquake. 
# syntax: plt.text(x, y, label)
plt.text(2000, 6000, '2010 Earthquake') # see note below

plt.show() 
```


![png](output_85_0.png)


With just a few lines of code, you were able to quickly identify and visualize the spike in immigration!

Quick note on x and y values in `plt.text(x, y, label)`:
    
     Since the x-axis (years) is type 'integer', we specified x as a year. The y axis (number of immigrants) is type 'integer', so we can just specify the value y = 6000.
    
```python
    plt.text(2000, 6000, '2010 Earthquake') # years stored as type int
```
    If the years were stored as type 'string', we would need to specify x as the index position of the year. Eg 20th index is year 2000 since it is the 20th year with a base year of 1980.
```python
    plt.text(20, 6000, '2010 Earthquake') # years stored as type int
```
    We will cover advanced annotation methods in later modules.

We can easily add more countries to line plot to make meaningful comparisons immigration from different countries. 

**Question:** Let's compare the number of immigrants from India and China from 1980 to 2013.


Step 1: Get the data set for China and India, and display dataframe.


```python
### type your answer here

df_CI = df_can.loc[['India', 'China'], years]
```

Double-click __here__ for the solution.
<!-- The correct answer is:
df_CI = df_can.loc[['India', 'China'], years]
df_CI.head()
-->

Step 2: Plot graph. We will explicitly specify line plot by passing in `kind` parameter to `plot()`.


```python
### type your answer here
df_CI.plot(kind='line')

plt.title('Immigration from CI')
plt.ylabel('Number of immigrants')
plt.xlabel('Years')

plt.show()


```


![png](output_92_0.png)


Double-click __here__ for the solution.
<!-- The correct answer is:
df_CI.plot(kind='line')
-->

That doesn't look right...

Recall that *pandas* plots the indices on the x-axis and the columns as individual lines on the y-axis. Since `df_CI` is a dataframe with the `country` as the index and `years` as the columns, we must first transpose the dataframe using `transpose()` method to swap the row and columns.


```python
df_CI = df_CI.transpose()
df_CI.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>India</th>
      <th>China</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1980</th>
      <td>8880</td>
      <td>5123</td>
    </tr>
    <tr>
      <th>1981</th>
      <td>8670</td>
      <td>6682</td>
    </tr>
    <tr>
      <th>1982</th>
      <td>8147</td>
      <td>3308</td>
    </tr>
    <tr>
      <th>1983</th>
      <td>7338</td>
      <td>1863</td>
    </tr>
    <tr>
      <th>1984</th>
      <td>5704</td>
      <td>1527</td>
    </tr>
  </tbody>
</table>
</div>



*pandas* will auomatically graph the two countries on the same graph. Go ahead and plot the new transposed dataframe. Make sure to add a title to the plot and label the axes.


```python
### type your answer here
df_CI.index = df_CI.index.map(int) 

df_CI.plot(kind='line')

plt.title('Immigration from CI')
plt.ylabel('Number of immigrants')
plt.xlabel('Years')

plt.show()


```


![png](output_97_0.png)


Double-click __here__ for the solution.
<!-- The correct answer is:
df_CI.index = df_CI.index.map(int) # let's change the index values of df_CI to type integer for plotting
df_CI.plot(kind='line')
-->

<!--
plt.title('Immigrants from China and India')
plt.ylabel('Number of Immigrants')
plt.xlabel('Years')
-->

<!--
plt.show()
--> 

From the above plot, we can observe that the China and India have very similar immigration trends through the years. 

*Note*: How come we didn't need to transpose Haiti's dataframe before plotting (like we did for df_CI)?

That's because `haiti` is a series as opposed to a dataframe, and has the years as its indices as shown below. 
```python
print(type(haiti))
print(haiti.head(5))
```
>class 'pandas.core.series.Series' <br>
>1980    1666 <br>
>1981    3692 <br>
>1982    3498 <br>
>1983    2860 <br>
>1984    1418 <br>
>Name: Haiti, dtype: int64 <br>

Line plot is a handy tool to display several dependent variables against one independent variable. However, it is recommended that no more than 5-10 lines on a single graph; any more than that and it becomes difficult to interpret.

**Question:** Compare the trend of top 5 countries that contributed the most to immigration to Canada.


```python
### type your answer here
df_can.sort_values(by='Total', ascending=False, axis=0, inplace=True)
df_top5 = df_can.head(5)
df_top5 = df_top5[years].transpose()
df_top5.index = df_top5.index.map(int)
df_top5.plot(kind='line') 
plt.title('Immigration Trend of Top 5 Countries')
plt.ylabel('Number of Immigrants')
plt.xlabel('Years')
plt.show()

```


![png](output_103_0.png)


Double-click __here__ for the solution.
<!-- The correct answer is:
\\ # Step 1: Get the dataset. Recall that we created a Total column that calculates the cumulative immigration by country. \\ We will sort on this column to get our top 5 countries using pandas sort_values() method.
\\ inplace = True paramemter saves the changes to the original df_can dataframe
df_can.sort_values(by='Total', ascending=False, axis=0, inplace=True)
-->

<!--
# get the top 5 entries
df_top5 = df_can.head(5)
-->

<!--
# transpose the dataframe
df_top5 = df_top5[years].transpose() 
-->

<!--
print(df_top5)
-->

<!--
\\ # Step 2: Plot the dataframe. To make the plot more readeable, we will change the size using the `figsize` parameter.
df_top5.index = df_top5.index.map(int) # let's change the index values of df_top5 to type integer for plotting
df_top5.plot(kind='line', figsize=(14, 8)) # pass a tuple (x, y) size
-->

<!--
plt.title('Immigration Trend of Top 5 Countries')
plt.ylabel('Number of Immigrants')
plt.xlabel('Years')
-->

<!--
plt.show()
-->

### Other Plots

Congratulations! you have learned how to wrangle data with python and create a line plot with Matplotlib. There are many other plotting styles available other than the default Line plot, all of which can be accessed by passing `kind` keyword to `plot()`. The full list of available plots are as follows:

* `bar` for vertical bar plots
* `barh` for horizontal bar plots
* `hist` for histogram
* `box` for boxplot
* `kde` or `density` for density plots
* `area` for area plots
* `pie` for pie plots
* `scatter` for scatter plots
* `hexbin` for hexbin plot

### Thank you for completing this lab!

This notebook was originally created by [Jay Rajasekharan](https://www.linkedin.com/in/jayrajasekharan) with contributions from [Ehsan M. Kermani](https://www.linkedin.com/in/ehsanmkermani), and [Slobodan Markovic](https://www.linkedin.com/in/slobodan-markovic).

This notebook was recently revised by [Alex Aklson](https://www.linkedin.com/in/aklson/). I hope you found this lab session interesting. Feel free to contact me if you have any questions!

This notebook is part of a course on **Coursera** called *Data Visualization with Python*. If you accessed this notebook outside the course, you can take this course online by clicking [here](http://cocl.us/DV0101EN_Coursera_Week1_LAB1).

<hr>

Copyright &copy; 2019 [Cognitive Class](https://cognitiveclass.ai/?utm_source=bducopyrightlink&utm_medium=dswb&utm_campaign=bdu). This notebook and its source code are released under the terms of the [MIT License](https://bigdatauniversity.com/mit-license/).
