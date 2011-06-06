RTC Python Library
==================
Requirements
------------
python >= 2.6

Example Usage
-------------
You can specify the sections you want to pull from the RESTful API.
> sections = ('bill_id', 'sponsor', 'committees')
> result = Bill.get_bill(bill_id='hr3-112', sections=sections)
> print bill.sponsor_id, bill.vetoed, bill.last_action.text

Otherwise, it uses the default that's specified in the library.
> bill = RTC.Bill.get_bill(bill_id)
> print bill.sponsor_id, bill.vetoed, bill.last_action.text

See RTCtest.py for more examples usage.

Development Notes for Contributers:
----------------------------------
- dict2obj is a function that coverts the json converted dictionary into a usable object. Since much of the RTC API's fields are not guaranteed, this may help avoid the extra coding for keyErrors.  Additionally, it is convenient to use dot notation instead of dictionaries.

###Creating class methods for collections
- This is the basic structure:

>class Bill(rtc): #name of collection (eg: bills, videos, floor, updates)
>   """  __doc__ string goes here """
>    __help__ = RTC_helpers.BILL_HELPER #string created and imported from RTC_helpers.py   
>    ...
>    @classmethod
>    def actions(cls, bill_id, sections=('actions',)): 
>        """doc string goes here"""        
>        #'sections' parameter above specifies what default sections to pull from a collection
>        func = "bills"  #the collection were using (eg: bills.json)
>        params = {'bill_id': bill_id}
>        result = super(Bill, cls).get(func, params, sections)
>        bill = result.bills[0]
>        return [i for i in bill.actions]

- When writing a classmethod you can skip creating the obj and just return a dictionary instead.  Just specify 'make_obj=False' when declaring the classmethod parameters. 
>    ...
>    @classmethod
>    def titles(cls, bill_id, sections=('titles',), make_obj=False):
>    ...

### Other comments
- It'd be helpful to enter helper text into the RTC_helpers.py after writing a new classmethod
- It'd be helpful to write a test function in RTCtest.py after writing a new classmethod