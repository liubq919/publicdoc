```json

{
    "customAttrs": {
        "kubernetes": {
            "config": [
                {
                    "name": "config.yaml",
                    "path": "/app/resources/config.yaml"
                },
                {
                    "name": "config.conf",
                    "path": "/app/resources/config.conf"
                }
            ],
            "hpa": {
                "enabled": true,
                "settings": {
                    "tag": {
                        "max": 25,
                        "min": 10
                    },
                    "untag": {
                        "^dev$": {
                            "max": 3,
                            "min": 1
                        },
                        "^uat$": {
                            "max": 5,
                            "min": 1
                        }
                    }
                }
            },
            "resources": {
                "untag": {
                    "^dev$": {
                        "request": {
                            "cpu": 100,
                            "memory": 64
                        },
                        "limit": {
                            "cpu": 1000,
                            "memory": 1024
                        }
                    },
                    "^uat$": {
                        "request": {
                            "cpu": 100,
                            "memory": 64
                        },
                        "limit": {
                            "cpu": 1000,
                            "memory": 1024
                        }
                    },
                    "^lt$": {
                        "request": {
                            "cpu": 100,
                            "memory": 64
                        },
                        "limit": {
                            "cpu": 2000,
                            "memory": 4096
                        }
                    }
                },
                "tag": {
                    "request": {
                        "cpu": 100,
                        "memory": 64
                    },
                    "limit": {
                        "cpu": 2000,
                        "memory": 4096
                    }
                }
            },
            "env": {
                "tag": [
                    {
                        "name": "JAVA_TOOL_OPTIONS",
                        "value": "-Xms1024M -Xmx2560M"
                    }
                ],
                "untag": {
                    "^dev$": [
                        {
                            "name": "JAVA_TOOL_OPTIONS",
                            "value": "-Xms1024M -Xmx2560M"
                        }
                    ],
                    "^uat$": [
                        {
                            "name": "JAVA_TOOL_OPTIONS",
                            "value": "-Xms1024M -Xmx2560M"
                        }
                    ],
                    "^lt$": [
                        {
                            "name": "JAVA_TOOL_OPTIONS",
                            "value": "-Xms1024M -Xmx2560M"
                        }
                    ]
                }
            },
            "args": {
                "untag": {
                    "^uat$": [
                        "-Dserver.port=8080",
                        "-args2=2"
                    ]
                },
                "tag": [
                        "-Dserver.port=8080",
                        "-args2=2"
                ]
            },
            "command": {
                "untag": {
                    "^uat$": [
                        "java"
                    ]
                },
                "tag": [
                    "java"
                ]
            },
            "port": 8080,
            "healthCheckUri": "/actuator/health",
            "initialDelaySeconds": 60
        },
        "istio": {
            "vs": {
                "untag": {
                    "^dev$": [
                        {
                            "retries": {
                                "attempts": 3,
                                "perTryTimeout": "5s",
                                "retryOn": "gateway-error,connect-failure,refused-stream,5xx"
                            },
                            "route": [
                                {
                                    "destination": {
                                        "host": "svc-a",
                                        "port": {
                                            "number": 80
                                        },
                                        "subset": "main"
                                    }
                                }
                            ]
                        }
                    ],
                    "^feature(-[a-z0-9]+)+$": [
                        {
                            "retries": {
                                "attempts": 3,
                                "perTryTimeout": "5s",
                                "retryOn": "gateway-error,connect-failure,refused-stream,5xx"
                            },
                            "route": [
                                {
                                    "destination": {
                                        "host": "svc-a",
                                        "port": {
                                            "number": 80
                                        },
                                        "subset": "main"
                                    }
                                }
                            ]
                        }
                    ],
                    "^uat$": [
                        {
                            "retries": {
                                "attempts": 3,
                                "perTryTimeout": "5s",
                                "retryOn": "gateway-error,connect-failure,refused-stream,5xx"
                            },
                            "route": [
                                {
                                    "destination": {
                                        "host": "svc-a",
                                        "port": {
                                            "number": 80
                                        },
                                        "subset": "main"
                                    }
                                }
                            ]
                        }
                    ],
                    "^hotfix-([0-9]+)(.[0-9]+)*$": [
                        {
                            "retries": {
                                "attempts": 3,
                                "perTryTimeout": "5s",
                                "retryOn": "gateway-error,connect-failure,refused-stream,5xx"
                            },
                            "route": [
                                {
                                    "destination": {
                                        "host": "svc-a",
                                        "port": {
                                            "number": 80
                                        },
                                        "subset": "main"
                                    }
                                }
                            ]
                        }
                    ],
                    "^hotfix-v([0-9]+)(.[0-9]+)*$": [
                        {
                            "retries": {
                                "attempts": 3,
                                "perTryTimeout": "5s",
                                "retryOn": "gateway-error,connect-failure,refused-stream,5xx"
                            },
                            "route": [
                                {
                                    "destination": {
                                        "host": "svc-a",
                                        "port": {
                                            "number": 80
                                        },
                                        "subset": "main"
                                    }
                                }
                            ]
                        }
                    ]
                },
                "tag": [
                    {
                        "retries": {
                            "attempts": 3,
                            "perTryTimeout": "5s",
                            "retryOn": "gateway-error,connect-failure,refused-stream,5xx"
                        },
                        "route": [
                            {
                                "destination": {
                                    "host": "svc-a",
                                    "port": {
                                        "number": 80
                                    },
                                    "subset": "main"
                                }
                            }
                        ]
                    }
                ]
            },
            "dr": {
                "untag": {
                    "^dev$": {
                        "host": "svc-a",
                        "subsets": [
                            {
                                "labels": {
                                    "app": "svc-a"
                                },
                                "name": "main"
                            }
                        ],
                        "trafficPolicy": {
                            "loadBalancer": {
                                "simple": "LEAST_CONN"
                            }
                        }
                    },
                    "^feature(-[a-z0-9]+)+$": {
                        "host": "svc-a",
                        "subsets": [
                            {
                                "labels": {
                                    "app": "svc-a"
                                },
                                "name": "main"
                            }
                        ],
                        "trafficPolicy": {
                            "loadBalancer": {
                                "simple": "LEAST_CONN"
                            }
                        }
                    },
                    "^uat$": {
                        "host": "svc-a",
                        "subsets": [
                            {
                                "labels": {
                                    "app": "svc-a"
                                },
                                "name": "main"
                            }
                        ],
                        "trafficPolicy": {
                            "loadBalancer": {
                                "simple": "LEAST_CONN"
                            }
                        }
                    },
                    "^hotfix-v([0-9]+)(.[0-9]+)*$": {
                        "host": "svc-a",
                        "subsets": [
                            {
                                "labels": {
                                    "app": "svc-a"
                                },
                                "name": "main"
                            }
                        ],
                        "trafficPolicy": {
                            "loadBalancer": {
                                "simple": "LEAST_CONN"
                            }
                        }
                    },
                    "^hotfix-([0-9]+)(.[0-9]+)*$": {
                        "host": "svc-a",
                        "subsets": [
                            {
                                "labels": {
                                    "app": "svc-a"
                                },
                                "name": "main"
                            }
                        ],
                        "trafficPolicy": {
                            "loadBalancer": {
                                "simple": "LEAST_CONN"
                            }
                        }
                    }
                },
                "tag": {
                    "host": "svc-a",
                    "subsets": [
                        {
                            "labels": {
                                "app": "svc-a"
                            },
                            "name": "main"
                        }
                    ],
                    "trafficPolicy": {
                        "loadBalancer": {
                            "simple": "LEAST_CONN"
                        }
                    }
                }
            }
        },
        "domains": {
            "untagged": {
                "^dev$": [
                    {
                        "domain": "svc-a-dev.example.com",
                        "accessType": "none"
                    }
                ],
                "^uat$": [
                    {
                        "domain": "svc-a-uat.example.com",
                        "accessType": "none"
                    }
                ],
                "^hotfix-([0-9]+)(.[0-9]+)*$": [
                    {
                        "domain": "svc-a-hotfix.example.com",
                        "accessType": "none"
                    }
                ]
            },
            "tagged": [
                {
                    "domain": "svc-a.example.com",
                    "accessType": "none"
                }
            ]
        }
    },
    "deployTemplate": "template_1",
    "enabled": true,
    "group": "example",
    "project": "svc-a"
}
```