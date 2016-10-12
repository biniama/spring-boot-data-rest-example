# spring-boot-data-rest-example
A sample spring boot project for learning spring boot data rest

Implementation of [https://spring.io/guides/gs/accessing-data-rest/]Accessing JPA Data with REST



# CURL requests
```json
[~]$ curl http://localhost:8080
{
  "_links" : {
    "people" : {
      "href" : "http://localhost:8080/people{?page,size,sort}",
      "templated" : true
    },
    "profile" : {
      "href" : "http://localhost:8080/profile"
    }
  }
}
[~]$ curl http://localhost:8080/profile
{
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/profile"
    },
    "people" : {
      "href" : "http://localhost:8080/profile/people"
    }
  }
}
[~]$ curl http://localhost:8080/people
{
  "_embedded" : {
    "people" : [ ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people"
    },
    "profile" : {
      "href" : "http://localhost:8080/profile/people"
    },
    "search" : {
      "href" : "http://localhost:8080/people/search"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 0,
    "totalPages" : 0,
    "number" : 0
  }
}
[~]$ curl -i -X POST -H "Content-Type:application/json" -d '{"firstName":"Biniam", "lastName":"Asnake"}' http://localhost:8080/people
HTTP/1.1 201
Location: http://localhost:8080/people/1
Content-Type: application/hal+json;charset=UTF-8
Transfer-Encoding: chunked
Date: Wed, 12 Oct 2016 06:02:18 GMT

{
  "firstName" : "Biniam",
  "lastName" : "Asnake",
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/1"
    },
    "person" : {
      "href" : "http://localhost:8080/people/1"
    }
  }
}
[~]$ curl http://localhost:8080/people
{
  "_embedded" : {
    "people" : [ {
      "firstName" : "Biniam",
      "lastName" : "Asnake",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/1"
        },
        "person" : {
          "href" : "http://localhost:8080/people/1"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people"
    },
    "profile" : {
      "href" : "http://localhost:8080/profile/people"
    },
    "search" : {
      "href" : "http://localhost:8080/people/search"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 1,
    "totalPages" : 1,
    "number" : 0
  }
}
[~]$ curl http://localhost:8080/people/1
{
  "firstName" : "Biniam",
  "lastName" : "Asnake",
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/1"
    },
    "person" : {
      "href" : "http://localhost:8080/people/1"
    }
  }
}
[~]$ curl http://localhost:8080/people/search
{
  "_links" : {
    "findByLastName" : {
      "href" : "http://localhost:8080/people/search/findByLastName{?name}",
      "templated" : true
    },
    "self" : {
      "href" : "http://localhost:8080/people/search"
    }
  }
}
[~]$ curl http://localhost:8080/people/search/findByLastName{?name=Asnake}
{
  "_embedded" : {
    "people" : [ {
      "firstName" : "Biniam",
      "lastName" : "Asnake",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/1"
        },
        "person" : {
          "href" : "http://localhost:8080/people/1"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/search/findByLastName?name=Asnake"
    }
  }
}
[~]$ curl http://localhost:8080/people/search/findByLastName{?name=Biniam}
{
  "_embedded" : {
    "people" : [ ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/search/findByLastName?name=Biniam"
    }
  }
}
[~]$ curl -i -X POST -H "Content-Type:application/json" -d '{"firstName":"Biniam Asnake", "lastName":"Kefale"}' http://localhost:8080/people/1
HTTP/1.1 404
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Date: Wed, 12 Oct 2016 06:06:54 GMT

{"timestamp":1476252414876,"status":404,"error":"Not Found","message":"No message available","path":"/people/1"}
[~]$ curl -i -X PUT -H "Content-Type:application/json" -d '{"firstName":"Biniam Asnake", "lastName":"Kefale"}' http://localhost:8080/people/1
HTTP/1.1 200
Location: http://localhost:8080/people/1
Content-Type: application/hal+json;charset=UTF-8
Transfer-Encoding: chunked
Date: Wed, 12 Oct 2016 06:07:10 GMT

{
  "firstName" : "Biniam Asnake",
  "lastName" : "Kefale",
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/1"
    },
    "person" : {
      "href" : "http://localhost:8080/people/1"
    }
  }
}
[~]$ curl http://localhost:8080/people/search/findByLastName{?name=Asnake}
{
  "_embedded" : {
    "people" : [ ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/search/findByLastName?name=Asnake"
    }
  }
}
[~]$ curl http://localhost:8080/people/search/findByLastName{?name=Kefale}
{
  "_embedded" : {
    "people" : [ {
      "firstName" : "Biniam Asnake",
      "lastName" : "Kefale",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/1"
        },
        "person" : {
          "href" : "http://localhost:8080/people/1"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/search/findByLastName?name=Kefale"
    }
  }
}
[~]$ curl -i -X POST -H "Content-Type:application/json" -d '{"firstName":"Ephrem Asnake", "lastName":"Kefale"}' http://localhost:8080/people
HTTP/1.1 201
Location: http://localhost:8080/people/2
Content-Type: application/hal+json;charset=UTF-8
Transfer-Encoding: chunked
Date: Wed, 12 Oct 2016 06:08:00 GMT

{
  "firstName" : "Ephrem Asnake",
  "lastName" : "Kefale",
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/2"
    },
    "person" : {
      "href" : "http://localhost:8080/people/2"
    }
  }
}
[~]$ curl http://localhost:8080/people
{
  "_embedded" : {
    "people" : [ {
      "firstName" : "Biniam Asnake",
      "lastName" : "Kefale",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/1"
        },
        "person" : {
          "href" : "http://localhost:8080/people/1"
        }
      }
    }, {
      "firstName" : "Ephrem Asnake",
      "lastName" : "Kefale",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/2"
        },
        "person" : {
          "href" : "http://localhost:8080/people/2"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people"
    },
    "profile" : {
      "href" : "http://localhost:8080/profile/people"
    },
    "search" : {
      "href" : "http://localhost:8080/people/search"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 2,
    "totalPages" : 1,
    "number" : 0
  }
}
[~]$ curl -i -X PATCH -H "Content-Type:application/json" -d '{"firstName":"Eph"}' http://localhost:8080/people/1
HTTP/1.1 200
Content-Type: application/hal+json;charset=UTF-8
Transfer-Encoding: chunked
Date: Wed, 12 Oct 2016 06:09:01 GMT

{
  "firstName" : "Eph",
  "lastName" : "Kefale",
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/1"
    },
    "person" : {
      "href" : "http://localhost:8080/people/1"
    }
  }
}
[~]$ curl http://localhost:8080/people
{
  "_embedded" : {
    "people" : [ {
      "firstName" : "Eph",
      "lastName" : "Kefale",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/1"
        },
        "person" : {
          "href" : "http://localhost:8080/people/1"
        }
      }
    }, {
      "firstName" : "Ephrem Asnake",
      "lastName" : "Kefale",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/2"
        },
        "person" : {
          "href" : "http://localhost:8080/people/2"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people"
    },
    "profile" : {
      "href" : "http://localhost:8080/profile/people"
    },
    "search" : {
      "href" : "http://localhost:8080/people/search"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 2,
    "totalPages" : 1,
    "number" : 0
  }
}
[~]$ curl -i -X DELETE http://localhost:8080/people/1
HTTP/1.1 204
Date: Wed, 12 Oct 2016 06:09:53 GMT

[~]$ curl http://localhost:8080/people
{
  "_embedded" : {
    "people" : [ {
      "firstName" : "Ephrem Asnake",
      "lastName" : "Kefale",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/2"
        },
        "person" : {
          "href" : "http://localhost:8080/people/2"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people"
    },
    "profile" : {
      "href" : "http://localhost:8080/profile/people"
    },
    "search" : {
      "href" : "http://localhost:8080/people/search"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 1,
    "totalPages" : 1,
    "number" : 0
  }
}
```
