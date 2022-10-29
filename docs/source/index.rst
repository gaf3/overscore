.. created by sphinxter
.. default-domain:: py

overscore
=========

.. toctree::
    :maxdepth: 1
    :glob:
    :hidden:

    self
    *

.. module:: overscore

Library for double underscore access notation

Overscore provides a way to retrieve (and store) multi-dimensional data using a single string
with double underscores.

Inspired by Django, the access string can be used as a routine argument or a URL parameter,
allowing for complex access within simple contexts::

    import overscore

    data = {
        "things": {
            "a": {
                "b": [
                    {
                        "1": "yep"
                    }
                ]
            }
        }
    }

    overscore.get(data, "things__a__b__0____1")
    # "yep"

All keys/indexes are separated by double underscores. Extra underscores dictate how to
parse that place in the path.

.. list-table:: Underscores and Behavior
    :header-rows: 1

    * - Underscores
      - Following
      - Meaning
      - Example
      - Equivalent
    * - 2
      - letters and numbers
      - key
      - a__b
      - ["a"]["b"]
    * - 2
      - numbers
      - index
      - a__1
      - ["a"][1]
    * - 3
      - numbers
      - negative index
      - a___2
      - ["a"][-2]
    * - 4
      - numbers
      - numerical key
      - a____3
      - ["a"]["3"]
    * - 5
      - numbers
      - neagtive numerical key
      - a_____4
      - ["a"]["-4"]

.. attribute:: NUMBER

    regex matching a number

.. attribute:: WORD

    regex matching a word

.. function:: get(data, path)

    Retrieves the value in multidimensional data at the double underscored path

    :param data: The multidimensional data
    :type data: dict or list or str
    :param path: The double underscored path to the intended value
    :type path: list or str

    **Usage**

    You can retrieve via a string::

        import overscore

        data = {
            "things": {
                "a": {
                    "b": [
                        {
                            "1": "yep"
                        }
                    ]
                }
            }
        }

        overscore.get(data, "things__a__b__0____1")
        # "yep"

    Or using via a list::

        overscore.get(data, ["things", "a", "b", 0, "1"])
        # "yep"

.. function:: set(data, path, value)

    Stores a value in multidimensional data at the double underscored path

    :param data: The multidimensional data
    :type data: dict or list or str
    :param path: The double underscored path to the intended value
    :type path: list or str
    :param value: The value to store

    **Usage**

    You can store via a string::

        import overscore

        data = {}

        overscore.set(data, "things__a__b___2____1", "yep")
        data
        # {
        #     "things": {
        #         "a": {
        #             "b": [
        #                 {
        #                     "1": "yep"
        #                 },
        #                 None
        #             ]
        #         }
        #     }
        # }

    Or using via a list::

        overscore.set(data, ["things", "a", "b", -2, "1"], "sure")
        data
        # {
        #     "things": {
        #         "a": {
        #             "b": [
        #                 {
        #                     "1": "sure"
        #                 },
        #                 None
        #             ]
        #         }
        #     }
        # }

.. function:: parse(text: str) -> list

    Parses text to a list of keys/indexes

    :param text: path to parse
    :type text: str
    :rtype: list

    **Usage**

    ::

        import overscore

        overscore.parse("a__0___1____2_____3")
        # [
        #     "a",
        #     0,
        #     -1,
        #     "2",
        #     "-3"
        # ]

.. function:: compile(path: list) -> str

    Compiles a list of keys/indexes to text

    :param path: The path to compile
    :type path: list
    :rtype: str

    **Usage**

    ::

        import overscore

        overscore.compile(["a", 0, -1, "2", "-3"])
        # "a__0___1____2_____3"

.. exception:: OverscoreError

    Used for any overscore issues encountered.
