### rest\_api.json

swagger 2.0 定义的 API 的模板。可以查看到所有 REST API 的接口信息。

```json
{
    "swagger": "2.0",
    "info": {
        "title": "Hyperledger Fabric API",
        "description": "Interact with the enterprise blockchain through Hyperledger Fabric API",
        "version": "1.0.0"
    },
    "host": "127.0.0.1:7050",
    "schemes": [
        "http"
    ],
    "produces": [
        "application/json"
    ],
    "paths": {
        "/chain": {
            "get": {
                "summary": "Blockchain information",
                "description": "The Chain endpoint returns information about the current state of the blockchain such as the height, the current block hash, and the previous block hash.",
                "tags": [
                    "Blockchain"
                ],
                "operationId": "getChain",
                "responses": {
                    "200": {
                        "description": "Blockchain information",
                        "schema": {
                           "$ref": "#/definitions/BlockchainInfo"
                        }
                    },
                    "default": {
                        "description": "Unexpected error",
                        "schema": {
                            "$ref": "#/definitions/Error"
                        }
                    }
                }
            }
        },
        "/chain/blocks/{Block}": {
            "get": {
                "summary": "Individual block information",
                "description": "The {Block} endpoint returns information about a specific block within the Blockchain. Note that the genesis block is block zero.",
                "tags": [
                    "Block"
                ],
                "operationId": "getBlock",
                "parameters": [{
                    "name": "Block",
                    "in": "path",
                    "description": "Block number to retrieve",
                    "type": "integer",
                    "format": "uint64",
                    "required": true
                }],
                "responses": {
                    "200": {
                        "description": "Individual Block contents",
                        "schema": {
                           "$ref": "#/definitions/Block"
                        }
                    },
                    "default": {
                        "description": "Unexpected error",
                        "schema": {
                            "$ref": "#/definitions/Error"
                        }
                    }
                }
            }
        },
        "/transactions/{ID}": {
            "get": {
                "summary": "Individual transaction contents",
                "description": "The /transactions/{ID} endpoint returns the transaction matching the specified TXID.",
                "tags": [
                    "Transactions"
                ],
                "operationId": "getTransaction",
                "parameters": [{
                    "name": "ID",
                    "in": "path",
                    "description": "Transaction to retrieve from the blockchain.",
                    "type": "string",
                    "required": true
                }],
                "responses": {
                    "200": {
                        "description": "Individual Transaction contents",
                        "schema": {
                           "$ref": "#/definitions/Transaction"
                        }
                    },
                    "default": {
                        "description": "Unexpected error",
                        "schema": {
                            "$ref": "#/definitions/Error"
                        }
                    }
                }
            }
        },
        "/devops/deploy": {
           "post": {
              "summary": "[DEPRECATED] Service endpoint for deploying Chaincode [DEPRECATED]",
              "description": "The /devops/deploy endpoint receives Chaincode deployment requests. The ChaincodTXand the required entities are first packaged into a container and subsequently deployed to the blockchain. If the Chaincode build and deployment are successful, a confirmation message is returned. Otherwise, an error is displayed alongside with a reason for the failure. This service endpoint is being deprecated, please use the /chaincode endpoint instead.",
              "tags": [
                  "Chaincode"
              ],
              "operationId": "chaincodeDeploy",
              "parameters": [{
                 "name": "ChaincodeSpec",
                 "in": "body",
                 "description": "Chaincode specification message",
                 "required": true,
                 "schema": {
                    "$ref": "#/definitions/ChaincodeSpec"
                 }
              }],
              "responses": {
                  "200": {
                      "description": "Successfully deployed chainCode",
                      "schema": {
                         "$ref": "#/definitions/OK"
                      }
                  },
                  "default": {
                      "description": "Unexpected error",
                      "schema": {
                          "$ref": "#/definitions/Error"
                      }
                  }
              }
           }
        },
        "/devops/invoke": {
           "post": {
              "summary": "[DEPRECATED] Service endpoint for invoking Chaincode functions [DEPRECATED]",
              "description": "The /devops/invoke endpoint receives requests for invoking functions in deployed Chaincodes. If the Chaincode function is invoked sucessfully, a transaction id is returned. Otherwise, an error is displayed alongside with a reason for the failure. This service endpoint is being deprecated, please use the /chaincode endpoint instead.",
              "tags": [
                  "Chaincode"
              ],
              "operationId": "chaincodeInvoke",
              "parameters": [{
                 "name": "ChaincodeInvocationSpec",
                 "in": "body",
                 "description": "Chaincode invocation message",
                 "required": true,
                 "schema": {
                    "$ref": "#/definitions/ChaincodeInvocationSpec"
                 }
              }],
              "responses": {
                  "200": {
                      "description": "Successfully invoked transaction",
                      "schema": {
                         "$ref": "#/definitions/OK"
                      }
                  },
                  "default": {
                      "description": "Unexpected error",
                      "schema": {
                          "$ref": "#/definitions/Error"
                      }
                  }
              }
           }
        },
        "/devops/query": {
           "post": {
              "summary": "[DEPRECATED] Service endpoint for querying Chaincode state [DEPRECATED]",
              "description": "The /devops/query endpoint receives requests to query Chaincode state. The request triggers a query method on the target Chaincode, both identified in the required payload. If the query method is successful, the response defined within the method is returned. Otherwise, an error is displayed alongside with a reason for the failure. This service endpoint is being deprecated, please use the /chaincode endpoint instead.",
              "tags": [
                  "Chaincode"
              ],
              "operationId": "chaincodeQuery",
              "parameters": [{
                 "name": "ChaincodeInvocationSpec",
                 "in": "body",
                 "description": "Chaincode invocation message",
                 "required": true,
                 "schema": {
                    "$ref": "#/definitions/ChaincodeInvocationSpec"
                 }
              }],
              "responses": {
                  "200": {
                      "description": "Successfully queried chaincode",
                      "schema": {
                         "$ref": "#/definitions/OK"
                      }
                  },
                  "default": {
                      "description": "Unexpected error",
                      "schema": {
                          "$ref": "#/definitions/Error"
                      }
                  }
              }
           }
        },
        "/chaincode": {
           "post": {
              "summary": "Service endpoint for Chaincode operations",
              "description": "The /chaincode endpoint receives requests to deploy, invoke, and query a target Chaincode. This service endpoint implements the JSON RPC 2.0 specification with the payload identifying the desired Chaincode operation within the 'method' field.",
              "tags": [
                  "Chaincode"
              ],
              "operationId": "chaincodeOp",
              "parameters": [{
                 "name": "ChaincodeOpPayload",
                 "in": "body",
                 "description": "Chaincode JSON RPC 2.0 payload",
                 "required": true,
                 "schema": {
                    "$ref": "#/definitions/ChaincodeOpPayload"
                 }
              }],
              "responses": {
                  "200": {
                      "description": "Chaincode operation successful",
                      "schema": {
                         "$ref": "#/definitions/ChaincodeOpSuccess"
                      }
                  },
                  "default": {
                      "description": "Chaincode operation failed",
                      "schema": {
                          "$ref": "#/definitions/ChaincodeOpFailure"
                      }
                  }
              }
           }
        },
        "/registrar": {
           "post": {
              "summary": "Register a user with the certificate authority",
              "description": "The /registrar endpoint receives requests to register a user with the certificate authority. The request must supply the registration id and password within the payload. If the registration is successful, the required transaction certificates are received and stored locally. Otherwise, an error is displayed alongside with a reason for the failure.",
              "tags": [
                  "Registrar"
              ],
              "operationId": "registerUser",
              "parameters": [{
                 "name": "Secret",
                 "in": "body",
                 "description": "User enrollment credentials",
                 "required": true,
                 "schema": {
                    "$ref": "#/definitions/Secret"
                 }
              }],
              "responses": {
                  "200": {
                      "description": "Successfully registered user with the certificate authority",
                      "schema": {
                         "$ref": "#/definitions/OK"
                      }
                  },
                  "default": {
                      "description": "Unexpected error",
                      "schema": {
                          "$ref": "#/definitions/Error"
                      }
                  }
              }
           }
        },
        "/registrar/{enrollmentID}": {
            "get": {
                "summary": "Confirm the user has registered with the certificate authority",
                "description": "The /registrar/{enrollmentID} endpoint confirms whether the specified user has registered with the certificate authority. If the user has registered, a confirmation message will be returned. Otherwise, an authorization failure will result.",
                "tags": [
                    "Registrar"
                ],
                "operationId": "getUserRegistration",
                "parameters": [{
                    "name": "enrollmentID",
                    "in": "path",
                    "description": "Username for which registration is to be confirmed",
                    "type": "string",
                    "required": true
                }],
                "responses": {
                    "200": {
                        "description": "Confirm registration for target user",
                        "schema": {
                           "$ref": "#/definitions/OK"
                        }
                    },
                    "default": {
                        "description": "Unexpected error",
                        "schema": {
                            "$ref": "#/definitions/Error"
                        }
                    }
                }
            },
            "delete": {
              "summary": "Delete user login tokens from local storage",
              "description": "The /registrar/{enrollmentID} endpoint deletes any existing client login tokens from local storage. After the completion of this request, the target user will no longer be able to execute transactions.",
              "tags": [
                  "Registrar"
              ],
                "operationId": "deleteUserRegistration",
                "parameters": [{
                    "name": "enrollmentID",
                    "in": "path",
                    "description": "Username for which login tokens are to be deleted",
                    "type": "string",
                    "required": true
                }],
                "responses": {
                    "200": {
                        "description": "Confirm deletion of user login tokens",
                        "schema": {
                           "$ref": "#/definitions/OK"
                        }
                    },
                    "default": {
                        "description": "Unexpected error",
                        "schema": {
                            "$ref": "#/definitions/Error"
                        }
                    }
                }
            }
        },
        "/registrar/{enrollmentID}/ecert": {
            "get": {
                "summary": "Retrieve user enrollment certificate",
                "description": "The /registrar/{enrollmentID}/ecert endpoint retrieves the enrollment certificate for a given user that has registered with the certificate authority. If the user has registered, a confirmation message will be returned containing the URL-encoded enrollment certificate. Otherwise, an error will result.",
                "tags": [
                    "Registrar"
                ],
                "operationId": "getUserEnrollmentCertificate",
                "parameters": [{
                    "name": "enrollmentID",
                    "in": "path",
                    "description": "EnrollmentID for which the certificate is requested",
                    "type": "string",
                    "required": true
                }],
                "responses": {
                    "200": {
                        "description": "Confirm registration for target user and return the URL-encoded enrollment certificate",
                        "schema": {
                           "$ref": "#/definitions/OK"
                        }
                    },
                    "default": {
                        "description": "Unexpected error",
                        "schema": {
                            "$ref": "#/definitions/Error"
                        }
                    }
                }
            }
        },
        "/registrar/{enrollmentID}/tcert": {
            "get": {
                "summary": "Retrieve user transaction certificates",
                "description": "The /registrar/{enrollmentID}/tcert endpoint retrieves the transaction certificates for a given user that has registered with the certificate authority. If the user has registered, a confirmation message will be returned containing an array of URL-encoded transaction certificates. Otherwise, an error will result. The desired number of transaction certificates is specified with the optional 'count' query parameter. The default number of returned transaction certificates is 1 and 500 is the maximum number of certificates that can be retrieved with a single request.",
                "tags": [
                    "Registrar"
                ],
                "operationId": "getUserTransactionCertificate",
                "parameters": [{
                    "name": "enrollmentID",
                    "in": "path",
                    "description": "EnrollmentID for which the certificate is requested",
                    "type": "string",
                    "required": true
                },
                {
                    "name": "count",
                    "in": "query",
                    "description": "The desired number of transaction certificates. The default number of returned transaction certificates is 1 and 500 is the maximum number of certificates that can be retrieved with a single request",
                    "type": "string"
                }],
                "responses": {
                    "200": {
                        "description": "Confirm registration for target user and return the desired number of URL-encoded transaction certificates",
                        "schema": {
                           "$ref": "#/definitions/OK"
                        }
                    },
                    "default": {
                        "description": "Unexpected error",
                        "schema": {
                            "$ref": "#/definitions/Error"
                        }
                    }
                }
            }
        },
        "/network/peers": {
            "get": {
                "summary": "List of network peers",
                "description": "The /network/peers endpoint returns a list of all existing network connections for the target peer node. The list includes both validating and non-validating peers.",
                "tags": [
                    "Network"
                ],
                "operationId": "getPeers",
                "responses": {
                    "200": {
                        "description": "List of network peers",
                        "schema": {
                           "$ref": "#/definitions/PeersMessage"
                        }
                    },
                    "default": {
                        "description": "Unexpected error",
                        "schema": {
                            "$ref": "#/definitions/Error"
                        }
                    }
                }
            }
        }
    },
    "definitions": {
        "BlockchainInfo": {
            "type": "object",
            "properties": {
                "height": {
                    "type": "integer",
                    "format": "uint64",
                    "description": "Current height of the blockchain."
                },
                "currentBlockHash": {
                    "type": "string",
                    "format": "bytes",
                    "description": "Hash of the last block in the blockchain."
                },
                "previousBlockHash": {
                    "type": "string",
                    "format": "bytes",
                    "description": "Hash of the previous block in the blockchain."
                }
            }
        },
        "Block": {
            "type": "object",
            "properties": {
                "proposerID": {
                    "type": "string",
                    "description": "Creator/originator of the block."
                },
                "timestamp": {
                  "$ref": "#/definitions/Timestamp",
                  "description": "Time of block creation."
                },
                "transactions": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Transaction"
                    }
                },
                "stateHash": {
                    "type": "string",
                    "format": "bytes",
                    "description": "Global state hash after executing all transactions in the block."
                },
                "previousBlockHash": {
                    "type": "string",
                    "format": "bytes",
                    "description": "Hash of the previous block in the blockchain."
                },
                "consensusMetadata": {
                  "type": "string",
                  "format": "bytes",
                  "description": "Metadata required for consensus."
                },
                "nonHashData": {
                  "type": "string",
                  "format": "bytes",
                  "description": "Data stored in the block, but excluded from the computation of block hash."
                }
            }
        },
        "Transaction": {
            "type": "object",
            "properties": {
                "type": {
                    "type": "string",
                    "default": "UNDEFINED",
                    "example": "UNDEFINED",
                    "enum":[
                        "UNDEFINED",
                        "CHAINCODE_DEPLOY",
                        "CHAINCODE_INVOKE",
                        "CHAINCODE_QUERY",
                        "CHAINCODE_TERMINATE"
                    ],
                    "description": "Transaction type."
                },
                "chaincodeID": {
                  "type": "string",
                  "format": "bytes",
                  "description": "Chaincode identifier as bytes."
                },
                "payload": {
                    "type": "string",
                    "format": "bytes",
                    "description": "Payload supplied for Chaincode function execution."
                },
                "id": {
                   "type": "string",
                   "description": "Unique transaction identifier."
                },
                "timestamp": {
                  "$ref": "#/definitions/Timestamp",
                  "description": "Time at which the chanincode becomes executable."
                },
                "confidentialityLevel": {
                    "$ref": "#/definitions/ConfidentialityLevel",
                    "description": "Confidentiality level of the Chaincode."
                },
                "nonce": {
                    "type": "string",
                    "format": "bytes",
                    "description": "Nonce value generated for this transaction."
                },
                "cert": {
                    "type": "string",
                    "format": "bytes",
                    "description": "Certificate of client sending the transaction."
                },
                "signature": {
                    "type": "string",
                    "format": "bytes",
                    "description": "Signature of client sending the transaction."
                }
            }
        },
        "ChaincodeID": {
            "type": "object",
            "properties": {
                "path": {
                    "type": "string",
                    "description": "Chaincode location in the file system. This value is required by the deploy transaction."
                },
                "name": {
                    "type": "string",
                    "description": "Chaincode name identifier. This value is required by the invoke and query transactions."
                }
            }
        },
        "ChaincodeSpec": {
            "type": "object",
            "properties": {
                "type": {
                    "type": "string",
                    "default": "GOLANG",
                    "example": "GOLANG",
                    "enum":[
                        "UNDEFINED",
                        "GOLANG",
                        "NODE",
                        "JAVA"
                    ],
                    "description": "Chaincode specification language."
                },
                "chaincodeID": {
                    "$ref": "#/definitions/ChaincodeID",
                    "description": "Unique Chaincode identifier."
                },
                "ctorMsg": {
                    "$ref": "#/definitions/ChaincodeInput",
                    "description": "Specific function to execute within the Chaincode."
                },
                "secureContext": {
                    "type": "string",
                    "description": "Username when security is enabled."
                },
                "confidentialityLevel": {
                    "$ref": "#/definitions/ConfidentialityLevel",
                    "description": "Confidentiality level of the Chaincode."
                }
            }
        },
        "ChaincodeInvocationSpec": {
            "type": "object",
            "properties": {
                "chaincodeSpec": {
                    "$ref": "#/definitions/ChaincodeSpec",
                    "description": "Chaincode specification message."
                }
            }
        },
        "ChaincodeOpPayload": {
           "type": "object",
           "properties": {
              "jsonrpc": {
                 "type": "string",
                 "default": "2.0",
                 "description": "A string specifying the version of the JSON-RPC protocol. Must be exactly '2.0'."
              },
              "method": {
                 "type": "string",
                 "description": "A string containing the name of the method to be invoked. Must be 'deploy', 'invoke', or 'query'."
              },
              "params": {
                  "$ref": "#/definitions/ChaincodeSpec",
                  "description": "A required Chaincode specification message identifying the target chaincode."
              },
              "id": {
                 "type": "integer",
                 "format": "int64",
                 "description": "An integer number used to correlate the request and response objects. If it is not included, the request is assumed to be a notification and the server will not generate a response."
              }
           },
           "required": [
              "jsonrpc",
              "method",
              "params",
              "id"
           ]
        },
        "ConfidentialityLevel":{
            "type": "string",
            "default": "PUBLIC",
            "example": "PUBLIC",
            "enum":[
                "PUBLIC",
                "CONFIDENTIAL"
              ],
            "description": "Confidentiality level of the Chaincode."
        },
        "ChaincodeInput": {
            "type": "object",
            "properties": {
                "args": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    },
                    "description": "Arguments supplied to the Chaincode function."
                }
            }
        },
        "Secret": {
            "type": "object",
            "properties": {
                "enrollId": {
                    "type": "string",
                    "description": "User enrollment id registered with the certificate authority."
                },
                "enrollSecret": {
                    "type": "string",
                    "description": "User enrollment password registered with the certificate authority."
                }
            }
        },
        "Timestamp": {
            "type": "object",
            "properties": {
                "seconds": {
                    "type": "integer",
                    "format": "int64",
                    "description": "Represents seconds of UTC time since Unix epoch 1970-01-01T00:00:00Z. Must be from from 0001-01-01T00:00:00Z to 9999-12-31T23:59:59Z inclusive."
                },
                "nanos": {
                    "type": "integer",
                    "format": "int32",
                    "description": "Non-negative fractions of a second at nanosecond resolution. Negative second values with fractions must still have non-negative nanos values that count forward in time. Must be from 0 to 999,999,999 inclusive."
                }
            }
        },
        "PeersMessage": {
            "type": "object",
            "properties": {
                "peers": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/PeerEndpoint"
                    }
                }
            }
        },
        "PeerEndpoint": {
            "type": "object",
            "properties": {
                "ID": {
                    "$ref": "#/definitions/PeerID",
                    "description": "Unique peer identifier."
                },
                "address": {
                    "type": "string",
                    "description": "ipaddress:port combination identifying a network peer."
                },
                "type": {
                    "type": "string",
                    "default": "UNDEFINED",
                    "example": "UNDEFINED",
                    "enum":[
                        "UNDEFINED",
                        "VALIDATOR",
                        "NON_VALIDATOR"
                    ],
                    "description": "Network peer type."
                },
                "pkiID": {
                    "type": "string",
                    "format": "bytes",
                    "description": "PKI identifier for the network peer."
                }
            }
        },
        "PeerID": {
            "type": "object",
            "properties": {
                "name": {
                    "type": "string",
                    "description": "Name which uniquely identifies a network peer."
                }
            }
        },
        "Error": {
            "type": "object",
            "properties": {
                "Error": {
                    "type": "string",
                    "description": "A descriptive message explaining the cause of error."
                }
            }
        },
        "OK": {
            "type": "object",
            "properties": {
                "OK": {
                    "type": "string",
                    "description": "A descriptive message confirming a successful request."
                },
                "message": {
                    "type": "string",
                    "description": "An optional parameter containing additional information about the request."
                }
            }
        },
        "ChaincodeOpSuccess": {
           "type": "object",
           "properties": {
              "jsonrpc": {
                 "type": "string",
                 "default": "2.0",
                 "description": "A string specifying the version of the JSON-RPC protocol. Must be exactly '2.0'."
              },
              "result": {
                  "$ref": "#/definitions/rpcResponse",
                  "description": "The value of this element is determined by the method invoked on the server."
              },
              "id": {
                  "type": "integer",
                  "format": "int64",
                  "default": 123,
                  "description": "This number will be the same as the value of the id member in the request object."
              }
           },
           "required": [
              "jsonrpc",
              "result",
              "id"
           ]
        },
        "ChaincodeOpFailure": {
           "type": "object",
           "properties": {
              "jsonrpc": {
                 "type": "string",
                 "default": "2.0",
                 "description": "A string specifying the version of the JSON-RPC protocol. Must be exactly '2.0'."
              },
              "error": {
                 "$ref": "#/definitions/rpcError",
                 "description": "A structured value specifying the code and description of the error that occurred."
             },
             "id": {
                 "type": "integer",
                 "format": "int64",
                 "default": 123,
                 "description": "This number will be the same as the value of the id member in the request object. If there was an error detecting the id in the request object (e.g. Parse error/Invalid Request), it will be null."
             }
          },
          "required": [
            "jsonrpc",
            "error",
            "id"
          ]
        },
        "rpcResponse": {
           "type": "object",
           "properties": {
              "Status": {
                 "type": "string",
                 "default": "OK",
                 "description": "A string confirming successful request execution."
              },
              "Message": {
                 "type": "string",
                 "default": "500",
                 "description": "Additional information about the response or values returned."
              }
           },
           "required": [
             "Status"
           ]
        },
        "rpcError": {
          "type": "object",
          "properties": {
            "code": {
              "type": "integer",
              "format": "int64",
              "default": -32700,
              "description": "A number that indicates the error type that occurred."
            },
            "message": {
              "type": "string",
              "default": "Parse error",
              "description": "A string providing a short description of the error."
            },
            "data": {
              "type": "string",
              "default": "Error unmarshalling chaincode request payload: unexpected end of JSON input",
              "description": "A primitive or structured value that contains additional information about the error (e.g. detailed error information, nested errors etc.)."
            }
          },
          "required": [
            "code",
            "message"
          ]
        }
    }
}

```

