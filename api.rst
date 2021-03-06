Offers API reference
====================

Summary
-------

Το domain που χρησιμοποιείται για όλες τις κλήσεις είναι το::

    https://offers.teiath.gr/api

το οποίο στις παρακάτω ενότητες αντικαθίσταται με το λεκτικό **<url>**.

Τα διαθέσιμα HTTP methods είναι τα εξής:

- **GET**
- **POST**
- **PUT**
- **DELETE**

Login
-----

====== =================== ===========
Method Rest URI            Description
====== =================== ===========
POST   url/**users/login** Login user in service.
====== =================== ===========

XML Request body::

    <User>
        <username>foo</username>
        <password>bar</password>
    </User>

JSON Request body::

    {
        "User": {
            "username": "foo",
            "passsword": "bar"
        }
    }

Sample XML request::

    $ curl -X POST https://offers.teiath.gr/api/users/login \
        -H "Content-Type: application/xml" \
        -H "Accept: application/xml" \
        -d "<User><username>foo</username><password>bar</password></User>" \
        -c /tmp/cookie

Sample JSON request::

    $ curl -X POST https://offers.teiath.gr/api/users/login \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"User": {"username": "foo", "password": "bar"}}' \
        -c /tmp/cookie

.. note::

    Τα παραπάνω request δημιουργούν ένα αρχείο cookies ``cookie`` στον κατάλογο ``tmp``.
    Χρησιμοποιείται για τα requests τα οποία απαιτούν αυθεντικοποίηση.

.. note::

    Χρησιμοποιήστε μόνο διπλά quotes ``"`` στο JSON. Εξωτερικά απαιτούνται μονά quotes ``'`` για αποφυγή expansion από το shell.

Sample JSON response on success::

    {
        "message": "Η αυθεντικοποίηση ολοκληρώθηκε με επιτυχία",
        "status_code": 200
    }

and on wrong credenials::

    {
        "message": "Δώστε έγκυρο όνομα και κωδικό χρήστη",
        "status_code": 403
    }


Offer Index (listing)
---------------------

====== ========================= ===========
Method Rest URI                  Description
====== ========================= ===========
GET    url/**offers**            Retrieve list of publicly available offers. By default returns 1st page of results only. Also see `Request options`_.
====== ========================= ===========

Sample JSON request::

    $ curl -X GET http://url/offers -H "Accept: application/json"

Sample JSON response::

    {
        "companies": [
            {
                "address": "οδός Ρόδων 45", 
                "afm": "011111111", 
                "created": "2012-04-17 09:24:38", 
                "fax": "2107654321", 
                "id": "4", 
                "image_count": "0", 
                "is_enabled": true, 
                "latitude": "37.94471", 
                "longitude": "23.76706", 
                "modified": "2012-04-17 09:24:38", 
                "municipality_id": "144", 
                "name": "Pizza Fan", 
                "phone": "2108112000", 
                "postalcode": "12345", 
                "service_type": "Υπηρεσίες",
                "user_id": "9"
            }
        ],
        "offers": [
            {
                "autoend": null, 
                "autostart": "2012-07-14 18:00:00", 
                "company_id": "4", 
                "coupon_count": "0", 
                "coupon_terms": "Εξαργύρωση μόνο μετά την λήξη", 
                "created": "2012-04-17 12:49:56", 
                "description": "40% για τους φοιτητές.", 
                "ended": null, 
                "id": "17", 
                "image_count": "0", 
                "is_spam": false, 
                "max_per_student": "0", 
                "modified": "2012-04-17 12:49:56", 
                "offer_category": "Φαγητό", 
                "offer_hours": [], 
                "offer_state": "active", 
                "offer_type": "limited", 
                "started": "2012-01-01 00:00:00", 
                "student_vote": {
                    "vote_type": 0
                }, 
                "student_coupon": {
                    "enabled": 0
                },
                "tags": "pizza fan πίτσα", 
                "title": "Pizza Fan 40 τοις εκατό", 
                "total_quantity": "0", 
                "vote_count": "0", 
                "vote_sum": "0"
            },
        ],
        "pagination": {
            "count": 157, 
            "current": 10, 
            "limit": 10, 
            "nextPage": true, 
            "options": {
                "conditions": []
            },
            "order": null, 
            "page": 1, 
            "pageCount": 16, 
            "paramType": "named", 
            "prevPage": false
        },
        "status_code": 200
    }

.. note::

    Για κάθε προσφορά επιστρέφονται και τα αντίστοιχα στοιχεία της επιχείρησης.
    Τα στοιχεία αυτά επιστρέφονται σε μια δεύτερη λίστα με το όνομα ``companies``.

.. note::

    * Το πεδίο ``ended`` συμπληρώνεται μετά το πέρας της προσφοράς.
    * Το πεδίο ``autoend`` δέν έχει νόημα για τις κατηγορίες προσφορών happy hour και coupons.
    * Το πεδίο ``autostart`` δέν έχει νόημα για την κατηγορία προσφορών happy hour.

.. note::

    Το πεδιο ``student_vote`` είναι διαθέσιμο μόνο σε σπουδαστές και μόνο μετά από σύνδεση στο σύστημα.

.. note::

    * Το πεδίο ``student_coupon`` είναι διαθέσιμο μόνο σε σπουδαστές, μετά απο σύνδεση στο σύστημα και 
      έχει νόημα μόνο για προσφορές τύπου ``coupons``.

    * Οι δυνατές τιμές είναι 0 και 1 οι οποίες υποδηλώνουν ότι ο χρήστης δεν μπορεί ή μπορεί να δεσμεύσει
      κουπόνι αντίστοιχα.

    * Σε άλλου τύπου προσφορές ή αν ο χρήστης δεν είναι συνδεδεμένος το πεδίο απουσιάζει

.. note::

    Οι τιμές των πεδίων `vote_type` συνοψίζονται στην ενότητα  `Offer Vote`_ .


Request options
---------------

Παράμετροι που χρησιμοποιούνται στα web requests.

============== =============== ===========
Sort keyword   Sort value      Description
============== =============== ===========
orderby        *recent*        Sort by recent additions
-------------- --------------- -----------
orderby        *rank*          Sort by vote results (sum of votes)
-------------- --------------- -----------
orderby        *votes*         Sort by vote number (count)
-------------- --------------- -----------
orderby        *distance*      Sort by user distance, available only when user coordinates are set
-------------- --------------- -----------
page           *<num>*         Show only results page number = *<num>*
============== =============== ===========

.. note::

    Από προεπιλογή επιστρέφεται μόνο η πρώτη σελίδα αποτελεσμάτων.

Sample JSON request with options::

    $ curl -X GET https://offers.teiath.gr/api/offers/index/orderby:rank/page:2 -H "Accept: application/json"


.. note::

    Για την χρήση παραμέτρων ταξινόμησης απαιτείται στο URI το **index** οταν ζητάμε λίστα όλων των προσφορών.


Offer Types
-----------

====== ======================== ===========
Method Rest URI                 Description
====== ======================== ===========
GET    url/**offers/happyhour** Retrieve list of publicly available **Happy Hour** offers.
------ ------------------------ -----------
GET    url/**offers/coupons**   Retrieve list of publicly available **Coupons** offers.
------ ------------------------ -----------
GET    url/**offers/limited**   Retrieve list of publicly available **Limited** offers.
====== ======================== ===========

Η απάντηση που επιστρέφουν τα παραπάνω URIs είναι αντίστοιχη με της ενότητας `Offer Index (listing)`_ .

.. note::

    Υποστηρίζονται όλες οι παράμετροι που αναφέρονται στην ενότητα `Request options`_.

Offer Categories
----------------

====== ====================================== ===========
Method Rest URI                               Description
====== ====================================== ===========
GET    url/**offers/category**/*{categoryId}* Retrieve offers for category with id: *{categoryId}*
====== ====================================== ===========

Η απάντηση που επιστρέφουν τα παραπάνω URIs είναι αντίστοιχη με της ενότητας `Offer Index (listing)`_ .

.. note::

    Υποστηρίζονται όλες οι παράμετροι που αναφέρονται στην ενότητα `Request options`_.


Offer View
----------

====== ========================== ===========
Method Rest URI                   Description
====== ========================== ===========
GET    url/**offer**/*{offerId}*  Retrieve info of offer with id *offerId*
====== ========================== ===========

The following table describes the URI parameters.

============== ========================== ===========
required parameters
-----------------------------------------------------
Parameter Name Data type                  Description
============== ========================== ===========
offerId        string                     ID of offer
============== ========================== ===========

Sample JSON request::

    $ curl -X GET http://url/offer/17 -H "Accept: application/json"


Sample JSON response (offer type **HappyHour**)::

    {
        "company": {
            "address": "οδός Ρόδων 45"", 
            "afm": "011111111", 
            "created": "2012-04-17 12:29:45", 
            "fax": "2107654321", 
            "id": "8", 
            "image_count": "0", 
            "is_enabled": true, 
            "latitude": "37.94471", 
            "longitude": "23.76706", 
            "modified": "2012-04-17 12:29:45", 
            "municipality_id": 144, 
            "name": "Άρωμα Βύνης", 
            "phone": "2100000000", 
            "postalcode": "12345", 
            "service_type": "Φαγητό", 
            "user_id": "13"
        }, 
        "offer": {
            "autoend": null, 
            "autostart": "2012-07-14 18:00:00", 
            "company_id": "8", 
            "coupon_count": "0", 
            "coupon_terms": "Εξαργύρωση μόνο μετά την λήξη",
            "created": "2012-04-17 12:35:51", 
            "description": "Βαρελίσια μπύρα 330ml ΜΟΝΟ 2,5€",
            "ended": null, 
            "id": "14", 
            "image_count": "0", 
            "is_spam": false, 
            "max_per_student": 5, 
            "modified": "2012-04-17 12:35:51", 
            "offer_category": "Φαγητό", 
            "offer_hours": [
                {
                    "day_id": "1", 
                    "ending1": "07:30:00", 
                    "ending2": "22:00:00", 
                    "starting1": "04:30:00", 
                    "starting2": "16:00:00"
                }, 
                {
                    "day_id": "2", 
                    "ending1": "22:00:00", 
                    "starting1": "06:00:00"
                }, 
                {
                    "day_id": "3", 
                    "ending1": "18:00:00", 
                    "ending2": "21:30:00", 
                    "starting1": "16:30:00", 
                    "starting2": "18:30:00"
                }, 
                {
                    "day_id": "4", 
                    "ending1": "14:00:00", 
                    "starting1": "02:00:00"
                }
            ], 
            "offer_state": "active", 
            "offer_type": "happy hour", 
            "started": "0000-00-00 00:00:00", 
            "student_vote": {
                "vote_type": 1
            }, 
            "tags": "άρωμα βύνης",
            "title": "Άρωμα Βύνης Happy Hours", 
            "total_quantity": 50, 
            "vote_count": "0", 
            "vote_sum": "0"
        }, 
        "status_code": 200
    }

Sample JSON response (offer type **Coupons**)::

    {
        "company": {
            "address": "test address 28", 
            "afm": "011111111", 
            "created": "2012-04-17 12:49:56", 
            "fax": "0987654321", 
            "id": "1", 
            "image_count": "1", 
            "is_enabled": true, 
            "latitude": "37.94471", 
            "longitude": "23.76706", 
            "modified": "2012-04-17 09:24:38", 
            "municipality_id": "13", 
            "name": "company_test_1", 
            "phone": "1234567890", 
            "postalcode": "12345", 
            "service_type": "estiatorio", 
            "user_id": "5"
        }, 
        "offer": {
            "autoend": null, 
            "autostart": "2012-07-14 18:00:00", 
            "company_id": "1", 
            "coupon_count": "0", 
            "coupon_terms": "", 
            "created": "2012-05-22 12:15:25", 
            "description": "100 κουπόνια για έκπτωση σε είδη γραφείου",
            "ended": null, 
            "id": "18", 
            "image_count": "0", 
            "is_spam": false, 
            "max_per_student": "0", 
            "modified": "2012-05-22 12:15:25", 
            "offer_category": "Προϊόντα", 
            "offer_hours": [], 
            "offer_state": "active", 
            "offer_type": "coupons", 
            "started": "2012-05-20 14:00:00", 
            "student_vote": {
                "vote_type": null
            }, 
            "student_coupon": {
                "enabled": 1
            },
            "tags": "γραφείο", 
            "title": "test", 
            "total_quantity": "100", 
            "vote_count": "0", 
            "vote_sum": "0"
        },
        "status_code": 200
    }

Sample JSON response (offer type **Limited**)::

    {
        "company": {
            "address": "οδός Ρόδων 45", 
            "afm": "011111111", 
            "created": "2012-04-17 09:24:38", 
            "fax": "2102345676", 
            "id": "4", 
            "image_count": "0", 
            "is_enabled": true, 
            "latitude": "37.94471", 
            "longitude": "23.76706", 
            "modified": "2012-04-17 09:24:38", 
            "municipality_id": "144", 
            "name": "Pizza Fan", 
            "phone": "2108112000", 
            "postalcode": "11122", 
            "service_type": "Υπηρεσίες", 
            "user_id": "9"
        }, 
        "offer": {
            "autoend": null, 
            "autostart": "2012-07-14 18:00:00", 
            "company_id": "4", 
            "coupon_count": "0", 
            "coupon_terms": null, 
            "created": "2012-04-17 12:49:56", 
            "description": "40% για τους φοιτητές.", 
            "ended": null, 
            "id": "17", 
            "image_count": "0", 
            "is_spam": false, 
            "max_per_student": 0, 
            "modified": "2012-04-17 12:49:56", 
            "offer_category": "Φαγητό", 
            "offer_hours": [], 
            "offer_state": "active", 
            "offer_type": "limited", 
            "started": "2012-01-01 00:00:00", 
            "tags": "pizza fan πίτσα", 
            "student_vote": {
                "vote_type": 0
            }, 
            "title": "Pizza Fan 40 τοις εκατό", 
            "total_quantity": null, 
            "vote_count": "0", 
            "vote_sum": "0"
        }, 
        "status_code": 200
    }


.. note::

    Οι τιμές των πεδίων `vote_type` συνοψίζονται στην ενότητα  `Offer Vote`_ .


Coupon View
-----------

====== =========================== ===========
Method Rest URI                    Description
====== =========================== ===========
GET    url/**coupon**/*{couponId}* Get coupon info with id *couponId*
====== =========================== ===========

Sample JSON request ::

    $ curl -X GET https://offers.teiath.gr/api/coupon/1 \
        -H "Accept: application/json" \
        -b /tmp/cookie

Sample JSON response ::

    {
        "company": {
            "address": "οδός Μαστραχά 88", 
            "afm": "000000012", 
            "fax": "2107654321", 
            "id": "101", 
            "latitude": "38.08804", 
            "longitude": "23.6598", 
            "name": "OPPENHEIM CAPITAL LTD", 
            "phone": "2101234567", 
            "postalcode": "12345", 
            "service_type": "Υπηρεσίες"
        }, 
        "coupon": {
            "created": "2012-06-14 10:06:50", 
            "id": "1", 
            "offer_id": "9", 
            "serial_number": "caccb026-2f2d-4d43-b70f-38e6d931cbd7", 
            "reinserted": "1",
            "student_id": "4"
        }, 
        "offer": {
            "autoend": "2077-01-01 00:00:00", 
            "autostart": "0000-00-00 00:00:00", 
            "company_id": "101", 
            "coupon_count": "1", 
            "coupon_terms": "Όροι εξαργύρωσης κουπονιού", 
            "description": "Περιγραφή προσφοράς 9", 
            "ended": "0000-00-00 00:00:00", 
            "id": "9", 
            "image_count": "0", 
            "is_spam": false, 
            "max_per_student": "5", 
            "offer_category_id": "5", 
            "offer_state_id": "2", 
            "offer_type_id": "2", 
            "started": "2012-01-01 00:00:00", 
            "tags": "λήμμα-31 λήμμα-1 λήμμα-4", 
            "student_vote": {
                "vote_type": 0
            }, 
            "title": "Προσφορά 9", 
            "total_quantity": "60", 
            "vote_count": "73", 
            "vote_sum": "-20"
        }, 
        "status_code": 200, 
        "student": {
            "firstname": "latsas", 
            "id": "4", 
            "lastname": "latsas", 
            "user_id": "151"
        }
    }


Sample XML response ::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="200">
      <offer id="9">
        <title>Προσφορά 9</title>
        <description>Περιγραφή προσφοράς Προσφορά 9</description>
        <started>2012-01-01T00:00:00</started>
        <ended>1970-01-01T02:00:00</ended>
        <autostart>1970-01-01T02:00:00</autostart>
        <autoend>1970-01-01T02:00:00</autoend>
        <coupon_terms>Όροι εξαργύρωσης κουπονιού</coupon_terms>
        <total_quantity>60</total_quantity>
        <coupon_count>1</coupon_count>
        <max_per_student>5</max_per_student>
        <tags>λήμμα-31 λήμμα-1 λήμμα-4 </tags>
        <offer_category_id>5</offer_category_id>
        <offer_type_id>2</offer_type_id>
        <company_id>101</company_id>
        <image_count>0</image_count>
        <offer_state_id>2</offer_state_id>
        <is_spam>0</is_spam>
        <vote_count>73</vote_count>
        <vote_sum>-20</vote_sum>
        <student_vote>
            <vote_type>0</vote_type>
        </student_vote>
      </offer>
      <coupon id="1">
        <serial_number>caccb026-2f2d-4d43-b70f-38e6d931cbd7</serial_number>
        <created>2012-06-14T10:06:50</created>
        <offer_id>9</offer_id>
        <student_id>4</student_id>
        <reinserted>1</reinserted>
      </coupon>
      <student id="4">
        <firstname>latsas</firstname>
        <lastname>latsas</lastname>
        <user_id>151</user_id>
      </student>
      <company id="101">
        <name>OPPENHEIM CAPITAL LTD</name>
        <address>οδός Μαστραχά 88</address>
        <postalcode>12345</postalcode>
        <phone>2101234567</phone>
        <fax>2107654321</fax>
        <service_type>Υπηρεσίες</service_type>
        <afm>000000012</afm>
        <latitude>38.08804</latitude>
        <longitude>23.6598</longitude>
      </company>
    </response>

.. note::

    - Για την ενέργεια απαιτείται αυθεντικοποίηση.
    - Η ενέργεια είναι διαθέσιμη μόνο σε σπουδαστές

.. note::

    Εάν το κουπόνι δεν υπάρχει επιστρέφεται HTTP 404.

Sample not found response ::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="404"><message>Not Found</message></response>

.. note::

    Το πεδίο `reinserted` υποδυκνείει εάν το κουπόνι έχει αποδεσμευθεί ή όχι.
    Η τιμή `1` δείχνει αποδεσμευμένο κουπόνι και η τιμή `0` ότι το κουπόνι είναι σε
    ισχύ και ο χρήστης είναι ακόμα ο κάτοχος του κουπονιού. Δες: `Coupon release (reinsert)`_ .

.. note::

    Οι τιμές των πεδίων `vote_type` συνοψίζονται στην ενότητα  `Offer Vote`_ .


Coupon Index
------------

====== =========================== ===========
Method Rest URI                    Description
====== =========================== ===========
GET    url/**coupons**             Get a list of user's coupons
====== =========================== ===========

Sample JSON request ::

    $ curl -s -X GET https://offers.teiath.gr/api/coupons \
        -H "Accept: application/json" \
        -b /tmp/cookie

Sample JSON response ::

    {
        "coupons": [
            {
                "coupon": {
                    "created": "2012-06-14 14:20:36", 
                    "id": "3", 
                    "serial_number": "0e9e3ae1-95a5-4e90-bd1a-7d5dd2cfd106",
                    "reinserted": "0"
                }, 
                "offer": {
                    "id": 100",
                    "company_id": "109", 
                    "coupon_terms": "Όροι εξαργύρωσης κουπονιού", 
                    "description": "Περιγραφή προσφοράς Προσφορά 39", 
                    "offer_category_id": "1", 
                    "offer_type_id": "2", 
                    "title": "Προσφορά 39", 
                    "vote_count": "62", 
                    "vote_sum": "7",
                    "is_spam": "0", 
                    "offer_state_id": "3",
                    "student_vote": {
                        "vote_type": null
                    }, 
                }
            }, 
            {
                "coupon": {
                    "created": "2012-06-14 14:20:30", 
                    "id": "2", 
                    "serial_number": "b5606315-b73a-49bf-ad91-f4a71f4642f9"
                }, 
                "offer": {
                    "id": 101",
                    "company_id": "122", 
                    "coupon_terms": "Όροι εξαργύρωσης κουπονιού", 
                    "description": "Περιγραφή προσφοράς Προσφορά 98", 
                    "offer_category_id": "8", 
                    "offer_type_id": "2", 
                    "title": "Προσφορά  98", 
                    "vote_count": "82", 
                    "vote_sum": "-55",
                    "is_spam": "1", 
                    "offer_state_id": "3",
                    "student_vote": {
                        "vote_type": null
                    }, 
                }
            }, 
            {
                "coupon": {
                    "created": "2012-06-14 10:06:50", 
                    "id": "1", 
                    "serial_number": "caccb026-2f2d-4d43-b70f-38e6d931cbd7"
                }, 
                "offer": {
                    "id": 102",
                    "company_id": "101", 
                    "coupon_terms": "Όροι εξαργύρωσης κουπονιού", 
                    "description": "Περιγραφή προσφοράς Προσφορά 9", 
                    "offer_category_id": "5", 
                    "offer_type_id": "2", 
                    "title": "Προσφορά 9", 
                    "vote_count": "73", 
                    "vote_sum": "-20",
                    "is_spam": "1", 
                    "offer_state_id": "3",
                    "student_vote": {
                        "vote_type": null
                    }, 
                }
            }
        ], 
        "status_code": 200
    }

Sample XML request ::

    $ curl -s -X GET https://offers.teiath.gr/api/coupons \
        -H "Accept: application/xml" \
        -b /tmp/cookie

Sample XML response::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="200">
      <coupons>
        <offer id="101">
          <title>Προσφορά 39</title>
          <description>Περιγραφή προσφοράς Προσφορά 39</description>
          <coupon_terms>Όροι εξαργύρωσης κουπονιού</coupon_terms>
          <offer_category_id>1</offer_category_id>
          <offer_type_id>2</offer_type_id>
          <vote_count>62</vote_count>
          <is_spam>0</is_spam>
          <offer_state_id>3</offer_state_id>
          <vote_sum>7</vote_sum>
          <company_id>109</company_id>
          <student_vote>
            <vote_type/>
          </student_vote>
        </offer>
        <coupon id="3">
          <serial_number>0e9e3ae1-95a5-4e90-bd1a-7d5dd2cfd106</serial_number>
          <created>2012-06-14T14:20:36</created>
        </coupon>
      </coupons>
      <coupons>
        <offer id="102">
          <title>Προσφορά 98</title>
          <description>Περιγραφή προσφοράς Προσφορά 98</description>
          <coupon_terms>Όροι εξαργύρωσης κουπονιού</coupon_terms>
          <offer_category_id>8</offer_category_id>
          <offer_type_id>2</offer_type_id>
          <vote_count>82</vote_count>
          <is_spam>1</is_spam>
          <offer_state_id>3</offer_state_id>
          <vote_sum>-55</vote_sum>
          <company_id>122</company_id>
          <student_vote>
            <vote_type/>
          </student_vote>
        </offer>
        <coupon id="2">
          <serial_number>b5606315-b73a-49bf-ad91-f4a71f4642f9</serial_number>
          <created>2012-06-14T14:20:30</created>
        </coupon>
      </coupons>
      <coupons>
        <offer id="103">
          <title>Προσφορά 9</title>
          <description>Περιγραφή προσφοράς Προσφορά 9</description>
          <coupon_terms>Όροι εξαργύρωσης κουπονιού</coupon_terms>
          <offer_category_id>5</offer_category_id>
          <offer_type_id>2</offer_type_id>
          <vote_count>73</vote_count>
          <vote_sum>-20</vote_sum>
          <company_id>101</company_id>
          <is_spam>1</is_spam>
          <offer_state_id>3</offer_state_id>
          <student_vote>
            <vote_type/>
          </student_vote>
        </offer>
        <coupon id="1">
          <serial_number>caccb026-2f2d-4d43-b70f-38e6d931cbd7</serial_number>
          <created>2012-06-14T10:06:50</created>
        </coupon>
      </coupons>
    </response>


.. note::

    - Για την ενέργεια απαιτείται αυθεντικοποίηση.
    - Η ενέργεια είναι διαθέσιμη μόνο σε σπουδαστές.
    - Επιστρέφονται τα κουπόνια του τρέχοντος σπουδαστή που έχει συνδεθεί, δεν απαιτείται κάποιο id.

.. note::

    Το πεδίο `reinserted` υποδυκνείει εάν το κουπόνι έχει αποδεσμευθεί ή όχι.
    Η τιμή `1` δείχνει αποδεσμευμένο κουπόνι και η τιμή `0` ότι το κουπόνι είναι σε
    ισχύ και ο χρήστης είναι ακόμα ο κάτοχος του κουπονιού. Δες: `Coupon release (reinsert)`_ .

.. note::

    Οι τιμές των πεδίων `vote_type` συνοψίζονται στην ενότητα  `Offer Vote`_ .

.. note::

    Το πεδίο `is_spam` παίρνει τιμές `1` και `0`.

.. note::

    Το πεδίο `offer_state_id` παίνει τις τιμές:

    - 1 => Προσφορά DRAFT (δεν έχει ενεργοποιηθεί ακόμα από τον επιχειρηματία).
    - 2 => Ενεργή προσφορά.
    - 3 => Προσφορά η οποία έχει λήξει.

    Οι παραπάνω τιμές αντιστοιχούν στις `default τιμές <http://git.edu.teiath.gr/coupons.git/tree/app/Config/configuration.php.default>`_ της εφαρμογής.


Grab Coupon
-----------

====== =========================== ===========
Method Rest URI                    Description
====== =========================== ===========
POST   url/**coupon**/*{offerId}*  Get coupon for offer with id *offerId*
====== =========================== ===========

Sample JSON request ::

    $ curl -X POST http://url/coupon/1 \
        -H "Accept: application/json" \
        -b /tmp/cookie

Sample JSON response::

    {
        "id": "9",
        "message": "Το κουπόνι δεσμεύτηκε επιτυχώς",
        "serial_number": "637d31b5-0760-4390-b164-0c2978f845d9",
        "status_code": 200
    }

Sample XML request ::

    $ curl -X POST http://url/coupon/1 \
        -H "Accept: application/xml" \
        -b /tmp/cookie

Sample XML response::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="200">
      <message>Το κουπόνι δεσμεύτηκε επιτυχώς</message>
      <id>10</id>
      <serial_number>d13f9aec-8d5c-4d52-90ef-794421f1b515</serial_number>
    </response>

Όταν ο σπουδαστής δεσμεύσει τον μέγιστο αριθμό κουπονιών επιστρέφεται **HTTP 400**.

XML::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="400">
      <message>Έχετε δεσμεύσει τον μέγιστο αριθμό κουπονιών για αυτήν την προσφορά.</message>
    </response>

JSON::

    {
        "message": "Έχετε δεσμεύσει τον μέγιστο αριθμό κουπονιών για αυτήν την προσφορά.",
        "status_code": 400
    }


.. note::

    - Για την ενέργεια απαιτείται αυθεντικοποίηση.
    - Η ενέργεια είναι διαθέσιμη μόνο σε σπουδαστές.

Όταν ο σπουδαστής δεν έχει αυθεντικοποιηθεί επιστρέφεται **HTTP 403**.

XML::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="403">
      <message>Forbidden</message>
    </response>

JSON::

    {
        "message": "Forbidden", 
        "status_code": "403"
    }

Όταν ο σπουδαστής δεσμεύσει τον μέγιστο αριθμό κουπονιών επιστρέφεται **HTTP 400**.


Coupon release (reinsert)
-------------------------

====== ======================================================== ===========
Method Rest URI                                                 Description
====== ======================================================== ===========
GET    url/**coupons/reinsert**/*{couponId}*                    Release coupon with id: *{couponId}*
====== ======================================================== ===========

Αποδεσμεύει το κουπόνι. Ο χρήστης παύει να είναι ο κάτοχος του κουπονιού και
η ισχύς του κουπονιού ακυρώνεται. Απαιτείται αυθεντικοποίηση.

Sample JSON request ::

    $ curl -X GET https://offers.teiath.gr/api/coupons/reinsert/10
        -H "Accept: application/json" \
        -b /tmp/cookie

Sample JSON response ::

    {
        "id": "10", 
        "message": "Το κουπόνι f617a0bf-e628-4823-92b7-1954286d934d αποδεσμεύτηκε επιτυχώς", 
        "serial_number": "f617a0bf-e628-4823-92b7-1954286d934d", 
        "status_code": 200
    }


Set coordinates
---------------

====== ======================================================== ===========
Method Rest URI                                                 Description
====== ======================================================== ===========
GET    url/**users/coordinates**/lat:{latitude}/lng:{longitude}  Set location to {latitude}, {longitude}
====== ======================================================== ===========

Sample JSON request ::

    $ curl -X GET http://url/users/coordinates/lat:55.496858/lng:9.747620 \
        -H "Accept: application/json" \
        -b /tmp/cookie

Sample JSON response::

    {
        "message": "Οι συντεταγμένες αποθηκεύτηκαν (23.312345,87.19325)",
        "status_code": 200
    }

Sample XML request ::

    $ curl -X GET http://url/users/coordinates/lat:55.496858/lng:9.747620 \
        -H "Accept: application/xml" \
        -b /tmp/cookie

Sample XML response::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="200">
        <message>Οι συντεταγμένες αποθηκεύτηκαν (23.312345,87.19325)</message>
    </response>


Set search radius
-----------------

====== ================================= ============
Method Rest URI                          Description
====== ================================= ============
GET    url/**users/radius**/{radius}     Set search radius to {radius} (in Km)
====== ================================= ============

Valid radius values:

+----------------+
| {radius} in Km |
+================+
| 1              |
+----------------+
| 5              |
+----------------+
| 10             |
+----------------+

Sample JSON request ::

    $ curl -X GET https://offers.teiath.gr/api/users/radius/5 \
        -H "Accept: application/json" \
        -b /tmp/cookie

Sample JSON response::

    {
        "status_code":200,
        "message":"Η ακτίνα αναζήτησης αποθηκεύτηκε με επιτυχία."
    }


Sample XML request ::

    $ curl -X GET https://offers.teiath.gr/api/users/radius/5 \
        -H "Accept: application/xml" \
        -b /tmp/cookie

Sample XML response::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="200">
        <message>Η ακτίνα αναζήτησης αποθηκεύτηκε με επιτυχία.</message>
    </response>

.. note ::

    Σε περίπτωση μη έγκυρου αριθμού ακτίνας, χρησιμοποιείται η μεγαλύτερη.


Offer Vote
----------

====== ==================================== ===========
Method Rest URI                             Description
====== ==================================== ===========
GET    url/**vote/vote_up**/*{offerId}*     Upvote the offer with id *{offerId}*
------ ------------------------------------ -----------
GET    url/**vote/vote_down**/*{offerId}*   Downvote the offer with id *{offerId}*
------ ------------------------------------ -----------
GET    url/**vote/vote_cancel**/*{offerId}* Cancel vote for offer with id *{offerId}*
====== ==================================== ===========


Sample JSON response for upvote::

    {
        "status_code": 200, 
        "vote": {
            "offer_id": "1", 
            "vote_type": 1
        }
    }

Sample XML response for upvote::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="200">
      <vote>
        <offer_id>1</offer_id>
        <vote_type>1</vote_type>
      </vote>
    </response>


Vote type table:

============ ====================================
vote type    interpretation
============ ====================================
**1**        upvote (+1 at web)
------------ ------------------------------------
**0**        downvote (-1 at web)
------------ ------------------------------------
**null**     student hasn't voted yet
============ ====================================


User Votes (listing)
--------------------

====== ==================================== ===========
Method Rest URI                             Description
====== ==================================== ===========
GET    url/**votes**                        Return logged-in user's votes
====== ==================================== ===========

Επιστρέφει όλες τις προσφορές που έχει ψηφίσει ο συνδεδεμένος σπουδαστής.
Η ψήφος του χρήστη για μια συγκεκριμένη προσφορά επιστρέφεται στο πεδίο `vote`
το οποίο μπορεί να περιέχει `1` (θετική ψήφος) ή `0` (αρνητική ψήφος).

.. note::

    Απαιτείται αυθεντικοποίηση.

Sample JSON request::

    $ curl -s -X GET 'https://offers.teiath.gr/api/votes/' \
        -H "Accept: application/json" -b /tmp/cookie


Sample JSON response::

    {
        "status_code": 200, 
        "votes": [
            {
                "offer": {
                    "id": "48", 
                    "offer_type_id": "2",
                    "title": "Προσφορά 48",
                    "vote_count": "81", 
                    "vote_minus": "44", 
                    "vote_plus": "37", 
                    "vote_sum": -7
                }, 
                "vote": 1
            }, 
            {
                "offer": {
                    "id": "264", 
                    "offer_type_id": "2",
                    "title": "Προσφορά 264",
                    "vote_count": "91", 
                    "vote_minus": "54", 
                    "vote_plus": "37", 
                    "vote_sum": -17
                }, 
                "vote": 1
            }
        ]
    }


Sample XML request::

    $ curl -s -X GET 'https://offers.teiath.gr/api/votes/' \
        -H "Accept: application/xml" -b /tmp/cookie


Sample XML response::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="200">
      <votes>
        <vote_info>
          <vote>1</vote>
          <offer>
            <id>48</id>
            <offer_type_id>2</offer_type_id>
            <title>Προσφορά 48</title>
            <vote_count>81</vote_count>
            <vote_plus>37</vote_plus>
            <vote_minus>44</vote_minus>
            <vote_sum>-7</vote_sum>
          </offer>
        </vote_info>
        <vote_info>
          <vote>1</vote>
          <offer>
            <id>264</id>
            <offer_type_id>2</offer_type_id>
            <title>Προσφορά 264</title>
            <vote_count>91</vote_count>
            <vote_plus>37</vote_plus>
            <vote_minus>54</vote_minus>
            <vote_sum>-17</vote_sum>
          </offer>
        </vote_info>
      </votes>
    </response>


Offer Statistics (counters)
---------------------------

====== ================================= ============
Method Rest URI                          Description
====== ================================= ============
GET    url/**offers/statistics**         Return taxonomy with offer counters
====== ================================= ============

Sample JSON request::

    curl -s -X GET https://offers.teiath.gr/api/offers/statistics \
        -H "Accept: application/json"

Sample JSON response::

    {
        "categories": [
            {
                "id": "1", 
                "name": "Φαγητό", 
                "offer_count": "23"
            }, 
            {
                "id": "2", 
                "name": "Υπηρεσίες", 
                "offer_count": "17"
            }, 
            {
                "id": "3", 
                "name": "Δραστηριότητες & Χόμπι",
                "offer_count": "17"
            }, 
            {
                "id": "4", 
                "name": "Ένδυση & Υπόδηση",
                "offer_count": "24"
            }, 
            {
                "id": "5", 
                "name": "Υγεία",
                "offer_count": "17"
            }, 
            {
                "id": "6", 
                "name": "Ταξίδια & Εκδρομές",
                "offer_count": "18"
            }, 
            {
                "id": "7", 
                "name": "Διασκέδαση",
                "offer_count": "20"
            }, 
            {
                "id": "8", 
                "name": "Προϊόντα",
                "offer_count": "21"
            }
        ], 
        "status_code": 200, 
        "total_offers": 157, 
        "my_stats": {
            "coupon_count": null, 
            "vote_count": null
        },
        "types": [
            {
                "id": 3, 
                "name": "limited", 
                "offer_count": "64"
            }, 
            {
                "id": 1, 
                "name": "happy hour", 
                "offer_count": "38"
            }, 
            {
                "id": 2, 
                "name": "coupons", 
                "offer_count": "55"
            }
        ]
    }

Sample XML response::

    <?xml version="1.0" encoding="UTF-8"?>
    <response status_code="200">
      <total_offers>102</total_offers>
      <types>
        <type>
          <id>1</id>
          <name>happy hour</name>
          <offer_count>42</offer_count>
        </type>
        <type>
          <id>2</id>
          <name>coupons</name>
          <offer_count>24</offer_count>
        </type>
        <type>
          <id>3</id>
          <name>limited</name>
          <offer_count>36</offer_count>
        </type>
      </types>
      <categories>
        <category>
          <type>
            <id>1</id>
            <name>happy hour</name>
            <offer_count>42</offer_count>
          </type>
          <type>
            <id>2</id>
            <name>coupons</name>
            <offer_count>24</offer_count>
          </type>
          <type>
            <id>3</id>
            <name>limited</name>
            <offer_count>36</offer_count>
          </type>
        </category>
      </categories>
      <my_stats>
        <coupon_count/>
        <vote_count/>
      </my_stats>
    </response>


.. note::

    Για να επιστραφεί το πλήθος των ψήφων και το πλήθος των κουπονιών του χρήστη, πρέπει να έχει
    επιτυχώς αυθεντικοποιηθεί στο σύστημα.


Πλήθος ψήφων/κουπονιών (JSON)::

    "my_stats": {
        "coupon_count": 9, 
        "vote_count": 4
    },


Πλήθος ψήφων/κουπονιών (XML)::

    <my_stats>
        <coupon_count>9</coupon_count>
        <vote_count>4</vote_count>
    </my_stats>



Search
------

====== ========================================== ============
Method Rest URI                                   Description
====== ========================================== ============
GET    url/**search**/*contains:{search-string}*  Return search results
====== ========================================== ============

Sample search using JSON::

    curl -X GET "http://url/search/contains:εμπόριο" \
        -H "Accept: application/json" -s

Response::

    {
        "companies": [
            {
                "address": "οδός Ρόδων 95",
                "afm": "000000012", 
                "created": "2012-04-17 12:49:56", 
                "fax": "2107654321", 
                "id": "101", 
                "image_count": "0", 
                "is_enabled": true, 
                "latitude": "37.94471", 
                "longitude": "23.76706", 
                "modified": "2012-07-23 12:02:58", 
                "municipality_id": "144", 
                "name": "CQ ΕΜΠΟΡΙΟ ΑΠΟΘΗΚΕΥΣΗ ΔΙΑΝΟΜΗ ΠΡΟΙΟΝΤΩΝ ΚΑΙ ΠΑΡΟΧΗ ΥΠΗΡΕΣΙΩΝ ΣΥΣΚΕΥΑΣΙΑΣ ΤΥΠΟΠΟΙΗΣΗΣ ΚΑΘΑΡΙΣΜΩΝ ΜΟΝΟΠΡΟΣΩΠΗ ΕΤΑΙΡΕΙΑ"
                "phone": "2101234567", 
                "postalcode": "12345", 
                "service_type": "Υπηρεσίες", 
                "user_id": "101"
            }
        ], 
        "offers": [
            {
                "autoend": null, 
                "autostart": null, 
                "company_id": "101", 
                "coupon_count": "0", 
                "coupon_terms": null, 
                "created": "2012-07-20 13:46:03", 
                "description": "happy hour test", 
                "ended": null, 
                "id": "275", 
                "image_count": "0", 
                "is_spam": false, 
                "max_per_student": null, 
                "modified": "2012-07-20 13:47:26", 
                "offer_category": "Φαγητό",
                "offer_hours": [
                    {
                        "day_id": "1", 
                        "ending1": "07:30:00", 
                        "ending2": "22:00:00", 
                        "starting1": "04:30:00", 
                        "starting2": "16:00:00"
                    }, 
                    {
                        "day_id": "2", 
                        "ending1": "22:00:00", 
                        "starting1": "06:00:00"
                    }, 
                    {
                        "day_id": "3", 
                        "ending1": "18:00:00", 
                        "ending2": "21:30:00", 
                        "starting1": "16:30:00", 
                        "starting2": "18:30:00"
                    }, 
                    {
                        "day_id": "4", 
                        "ending1": "14:00:00", 
                        "starting1": "02:00:00"
                    }
                ], 
                "offer_state": "active", 
                "offer_type": "happy hour", 
                "started": "2012-07-20 13:47:26", 
                "tags": "derp", 
                "title": "HH-test", 
                "total_quantity": null, 
                "vote_count": "0", 
                "vote_minus": "0", 
                "vote_plus": "0", 
                "vote_sum": "0"
            }, 

    [ ouput truncated ]
            { },
            { },
            { },
            { },
    [ ouput truncated ]
        ], 
        "pagination": {
            "count": 5, 
            "current": 5, 
            "limit": 10, 
            "nextPage": false, 
            "options": {
                "conditions": []
            }, 
            "order": null, 
            "page": 1, 
            "pageCount": 1, 
            "paramType": "named", 
            "prevPage": false
        }, 
        "status_code": 200
    }


.. note::

    Η αναζήτηση πραγματοποιείται στα εξής πεδία:

    Προσφορά:
        * title
        * description
        * tags

    Εταιρία:
        * name

.. note::

    To αποτέλεσμα της ανατήτησης έχει το ίδιο format με το αποτέλεσμα της ενότητας `Offer Index (listing)`_ .

