# Apache-Daffodil

## Description

This policy provides a way to build a REST API on top of the Apache Daffodil application.

Incubation Page for Application: https://daffodil.apache.org/

Definition From Apache Page: "The Data Format Description Language (DFDL) is a specification, developed by the Open Grid Forum, capable of describing many data formats, including both textual and binary, scientific and numeric, legacy and modern, commercial record-oriented, and many industry and military standards. It defines a language that is a subset of W3C XML schema to describe the logical format of the data, and annotations within the schema to describe the physical representation.

Daffodil is an open source implementation of the DFDL specification that uses these DFDL schemas to parse fixed format data into an infoset, which is most commonly represented as either XML or JSON. This allows the use of well-established XML or JSON technologies and libraries to consume, inspect, and manipulate fixed format data in existing solutions. Daffodil is also capable of the reverse by serializing or “unparsing” an XML or JSON infoset back to the original data format."


## Potential Next Steps

- Build a filter that implements Apache Daffodil directly in the API Gateway.
- Enable dynamic schmea/file delivery.

## Prerequisites

- ***Apache Daffodil:*** To make this policy work, you need to install the Apache Daffodil RPM on the server that is running the API Gateway. The user running the API Gateway will need access to the underlying application once installed, and the directories where the sample files and schema are stored.

##  Daffodil Policy

This policy was built with the intent of being a framework for utilizing Apache Daffodil within the Axway API Gateway.

- ***Starting Filter Placeholder - Not Required:*** This is an Extract REST Attributes filter. It is not required for this policy, but I often use it as the starting filter for policies, as it should always pass and has value for making additional attributes available if required.

- ***Grab File/Stream to Be Transformed:*** This filter is a placeholder for how we grab the file to be transformed. For this policy I am using the sample files for Apache Daffodil that are stored in the src repository and reading them from the system directory using the Execute Command process and a Linux "cat" to read and return the file. The filter uses the "file" query params value passed in to select the file name, enabling this to be a REST based solution. This would allow a seperate process to create and drop the files in the directory you are calling that could be transformed. The file could also be the payload of the message, and the next filter would just reference the content.body attribute instead.


- ***Set Data to be Transformed:*** This Set Attribute filter stores the file in a variable to ensure it can be handled as a string.

- ***Parse Input Against Schema via CLI RPM Call:*** This Execute Command filter calls the underlying Apache Daffodil RPM with the file and schema being provided by dynamic REST Attributes to reference these files on the command line. A sample request and schema are stored in the /src file of this github repo. This type of REST-ification of command line utilities can be used to securely expose any CLI application. For example openssl certificate generation. This filter will return the result of the transformed file.

- ***Respond with Original File and Transformation and Reflect:*** This sets a response that shows the origianl file and the transformation.

For questions or comments: dwille@axway.com
