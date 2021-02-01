---
description: >-
  This page is about testing Belt callback code locally with the Belt Extractor
  Python package
---

# Local Belt Testing

### Introduction

With Granary 1.0 Aretha the Belt Extractor is packaged and served in PIP package. This allows users to write tests against their custom callback code.

### Prerequisites

* Python 3.7 or 3.8 installed
* PIP installed \(20.2.2+\)
* Access to the PIP Artifactory of Granary

{% hint style="info" %}
To get access to the Artifactory, please get in touch.
{% endhint %}

### Setting The Stage

In an empty folder create a file called `callback.py` with your custom Belt Extractor callback code.

For this example let us assume the callback looks like this.

```python
from grnry_belt.models.update import Update

def execute(headers, payload, profile=None):
    # This should throw an exception
    correlation_id = payload['correlation_id']
    update = Update(correlation_id, ['path'], operation='_set').set_value('Test')
    return [update]

```

On the same level create a new file called `requirements.txt` with this content.

```text
grnry-belt==1.0.0
pytest
```

To proceed with testing install the needed dependencies for example with:

```bash
pip install -r requirements.txt --extra-index-url https://$PIP_USERNAME:$PIP_PASSWORD@artifactory.syncier.cloud/artifactory/api/pypi/analytics-pypi-release/simple
```

Replace `PIP_USERNAME` and `PIP_PASSWORD` with the proper values provided.

### Testing The Callback

Now that everything is setup, add a file `pytest.py` on root level containing the first tests.

```python
import unittest
from unittest import TestCase
from unittest.mock import patch
import callback


class TestCallback(TestCase):

    def setUp(self):
        print('setup Test Case')

    def test_callback_raises_missing_key(self):
        self.assertRaises(TypeError, callback.execute, None, None)

    def test_callback(self):
        return_value = callback.execute(None, {'correlation_id': '123'})
        self.assertEqual(return_value[0].get_id(), '123')


if __name__ == '__main__':
    unittest.main()

```

These tests can be run with the command `python -m pytest` . The output in the Terminal should look like this

```bash
$ python -m pytest 
================ test session starts ================
platform darwin -- Python 3.8.3, pytest-5.4.3, py-1.9.0, pluggy-0.13.1
rootdir: <dir>
collected 2 items                                   

test_callback.py ..                           [100%]
================= 2 passed in 0.08s =================
```

Congratulations! Now it is time to write some real tests!

### Advanced Usage

With the `grnry-belt`  package installed Granary provides access to all methods used in Belt Extractor itself. For testing, more advanced developers now have access to utility functions or the Kafka Consumer / Producer implementations. Be aware that these might change with over upcoming releases. 

### Further Information

* [https://www.python.org/downloads/](https://www.python.org/downloads/)
* [https://packaging.python.org/tutorials/installing-packages/](https://packaging.python.org/tutorials/installing-packages/)
* [https://docs.pytest.org/en/stable/](https://docs.pytest.org/en/stable/)

