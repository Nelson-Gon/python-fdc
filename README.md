# pyfdc: A python interface to FoodDataCentral
![Travis Build](https://travis-ci.com/Nelson-Gon/pyfdc.svg?branch=master)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Nelson-Gon/pyfdc/graphs/commit-activity)
[![PyPI version fury.io](https://badge.fury.io/py/pyfdc.svg)](https://pypi.python.org/pypi/pyfdc/)
[![PyPI license](https://img.shields.io/pypi/l/pyfdc.svg)](https://pypi.python.org/pypi/pyfdc/)
[![PyPI download Month](https://img.shields.io/pypi/dm/pyfdc.svg)](https://pypi.python.org/pypi/pyfdc/)
[![PyPI download week](https://img.shields.io/pypi/dw/pyfdc.svg)](https://pypi.python.org/pypi/pyfdc/)
[![PyPI download day](https://img.shields.io/pypi/dd/pyfdc.svg)](https://pypi.python.org/pypi/pyfdc/)
[![Project Status](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active) 
 [![GitHub last commit](https://img.shields.io/github/last-commit/Nelson-Gon/pyfdc.svg)](https://github.com/Nelson-Gon/pyfdc/commits/master)
[![made-with-python](https://img.shields.io/badge/Made%20with-Python-1f425f.svg)](https://www.python.org/)
[![GitHub issues](https://img.shields.io/github/issues/Nelson-Gon/pyfdc.svg)](https://GitHub.com/Nelson-Gon/pyfdc/issues/)
[![GitHub issues-closed](https://img.shields.io/github/issues-closed/Nelson-Gon/pyfdc.svg)](https://GitHub.com/Nelson-Gon/pyfdc/issues?q=is%3Aissue+is%3Aclosed)



----

**Installation**

The simplest way to install the latest release is as follows:

```
pip install pyfdc

```

To install the development version:


Open the Terminal/CMD/Git bash/shell and enter

```

python -m pip install git+https://github.com/Nelson-Gon/pyfdc.git@develop

# or for the less stable dev version
python -m pip install git+https://github.com/Nelson-Gon/pyfdc.git@develop

```

Otherwise:

```
# clone the repo
git clone https://www.github.com/Nelson-Gon/pyfdc.git
cd pyfdc
python3 setup.py install

```
---

**Sample usage**

```
from pyfdc import *

```

**Set session api key**

To avoid providing an api key for each call, one can set a session api key as follows:

```

utils.set_api_key("my_api_key_here")


```


**Key Features**

There are two key classes defined in `pyfdc`: 

1. `FoodSearch` implements the class for objects aimed at querying the database with a search term.
To get details about foods for a given search term, one can do the following:

```
my_search = pyfdc.FoodSearch(search_phrase="nugget")
list(my_search.get_food_info(target="fdcId"))

```

The above will result in the following output(truncated):

```

[[337348], [337394], [170725], [340673], [337347], [173721], [173722], [337346].....]]


```

To get descriptions of the different results:


```

list(my_search.get_food_info(target="description"))


```

This will result in the following result(truncated):

```

[['Chicken nuggets'], ['Turkey, nuggets'], ["WENDY'S, Chicken Nuggets"], ['Nutty Nuggets, Ralston Purina']]]


```

The simplest way to find out all available `targets` is to simply call:

```

list(my_search.get_food_info())


```

**This will throw an error showing what options are available.**:

```

target should be one of ['fdcId', 'description', 'scientificName', 'commonNames', 'additionalDescriptions', 'dataType', 'foodCode', 'gtinUpc', 'ndbNumber', 'publishedDate', 'brandOwner', 'ingredients', 'allHighlightFields', 'score']

```

For more details, please see the documentation of each of these classes and the
associated documents.

To get a `DataFrame` from multiple target fields, we can use `get_multiple_details` as shown:

```
my_search.get_multiple_details(["fdcId","foodCode","description"])
     fdcId  foodCode                                        description
0   337348  24198740                                    Chicken nuggets
1   337394  24208000                                    Turkey, nuggets
2   170725  57316200                           WENDY'S, Chicken Nuggets
3   340673  24198735                      Nutty Nuggets, Ralston Purina
4   337347  24198730                 Chicken nuggets, from school lunch
5   173721  26100260            Salmon nuggets, breaded, frozen, heated
6   173722  13120310      Salmon nuggets, cooked as purchased, unheated
```

2. `FoodDetails`

The `FoodSearch` class has an important advantage: it can allow us to obtain
FoodDataCentral(fdcId) IDs using a simple search term. To get full details about a given 
fdcId, one can do the following:

```
my_details = pyfdc.FoodDetails(fdc_id=504905)
my_details.get_food_details("ingredients")

```

This will give us the following output(truncated):

```

MECHANICALLY SEPARATED CHICKEN, CHICKEN BROTH,

```

To get nutrient details, we can use the following which returns a list of all 
nutrient details. For brevity, only part of the first list item is shown.

```

list(my_details.get_nutrients())

[      id number                  name  rank unitName
 0   1079    291  Fiber, total dietary  1200        g
 1   1079    291  Fiber, total dietary  1200        g
 2   1079    291  Fiber, total dietary  1200        g
 3   1079    291  Fiber, total dietary  1200        g
 4   1079    291  Fiber, total dietary  1200        g
 5   1079    291  Fiber, total dietary  1200        g
 6   1079    291  Fiber, total dietary  1200        g

```

To return a merge of the above results, we can use `merge_food_nutrients` as follows:

```
my_details.merge_nutrient_results()
     number                          name  rank unitName
id                                                      
1079    291          Fiber, total dietary  1200        g
1079    291          Fiber, total dietary  1200        g
1079    291          Fiber, total dietary  1200        g
1079    291          Fiber, total dietary  1200        g
1079    291          Fiber, total dietary  1200        g
     ...                           ...   ...      ...
1258    606  Fatty acids, total saturated  9700        g
1258    606  Fatty acids, total saturated  9700        g
1258    606  Fatty acids, total saturated  9700        g
1258    606  Fatty acids, total saturated  9700        g
1258    606  Fatty acids, total saturated  9700        g
[225 rows x 4 columns]

```
