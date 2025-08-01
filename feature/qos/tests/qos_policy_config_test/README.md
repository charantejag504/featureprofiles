# DP-1.2: QoS policy feature config

## Summary

Verify QoS policy feature configuration.

## Procedure

*   Connect DUT port-1 to ATE port-1, DUT port-2 to ATE port-2.

*   Classifiers config:

    *   Classifiers support both Ipv4 and IPv6 dscp range based classification.
        Classifiers will be applied to input interfaces.

        DSCP Range | Target group
        ---------- | ----------------
        0-3        | target-group-BE0
        4-7        | target-group-BE1
        8-11       | target-group-AF1
        16-19      | target-group-AF2
        24-27      | target-group-AF3
        32-35      | target-group-AF4
        48-59      | target-group-NC1

    *   Validate that the following values can be configured

        *   name
        *   type: Input_Classifier_Type_IPV4 or Input_Classifier_Type_IPV6
        *   term id
        *   IPv4 dscp-set
        *   IPV6 dscp-set
        *   Target-group

    *   The following OC config paths can be used to configure the above values:

        *   /qos/classifiers/classifier/config/name
        *   /qos/classifiers/classifier/config/type
        *   /qos/classifiers/classifier/terms/term/actions/config/target-group
        *   /qos/classifiers/classifier/terms/term/conditions/ipv4/config/dscp-set
        *   /qos/classifiers/classifier/terms/term/conditions/ipv6/config/dscp-set
        *   /qos/classifiers/classifier/terms/term/config/id

*   Queue config:

    *   Configure queue names:

        *   AF1
        *   AF2
        *   AF3
        *   AF4
        *   BE0
        *   BE1
        *   NC1

    *   The following OC config path can be used to configure the queue name:

        *   /qos/queues/queue/config/name

*   Forwarding-groups config:

    *   Configure forwarding-groups and output queue name.

        Output Queue | Fowarding group
        ------------ | --------------------
        BE0          | forwarding-group-BE0
        BE1          | forwarding-group-BE1
        AF1          | forwarding-group-AF1
        AF2          | forwarding-group-AF2
        AF3          | forwarding-group-AF3
        AF4          | forwarding-group-AF4
        NC1          | forwarding-group-NC1

    *   The following OC config paths can be used to configure the
        forwarding-groups:

        *   /qos/forwarding-groups/forwarding-group/config/name
        *   /qos/forwarding-groups/forwarding-group/config/output-queue

*   Scheduler-policies config:

    *   Schedulers define per queue for STRICT priority and weighted round
        robin. It will be applied to output interfaces.

        Queue | Priority | Sequence | Weight
        ----- | -------- | -------- | ------
        BE1   | not set  | 1        | 1
        BE0   | not set  | 1        | 2
        AF1   | not set  | 1        | 4
        AF2   | not set  | 1        | 8
        AF3   | not set  | 1        | 16
        AF4   | STRICT   | 0        | 100
        NC1   | STRICT   | 0        | 200

    *   Validate that the following values can be configured

        *   Scheduler-policy name
        *   Priority
            *   Note: Priority is set to STRICT for strict priority queues. Not
                set for round-robin queues.
        *   Sequence
        *   Input id
        *   Input-type: Input_InputType_QUEUE
        *   Queue
        *   Weight
            *   Note: For priority schedulers, this indicates the priority of
                the corresponding input. Higher values indicates higher
                priority. For weighted round-robin schedulers, this indicates
                the weight of the corresponding input.

    *   The following OC config paths can be used to configure the above values:

        *   /qos/scheduler-policies/scheduler-policy/config/name
        *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/config/sequence
        *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/config/type
        *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/id
        *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/input-type
        *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/queue
        *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/weight

*   Interfaces

    *   Validate that the following values can be configured on the output
        interfaces

        *   Queue name
        *   Scheduler-policy name

    *   Validate that the classifier can be configured on the input interfaces

        *   Classifier name

    *   The following OC config paths can be used to configure the above values:

        *   /qos/forwarding-groups/forwarding-group/config/name
        *   /qos/forwarding-groups/forwarding-group/config/output-queue

## Config parameter coverage

*   Classifiers

    *   /qos/classifiers/classifier/config/name
    *   /qos/classifiers/classifier/config/type
    *   /qos/classifiers/classifier/terms/term/actions/config/target-group
    *   /qos/classifiers/classifier/terms/term/conditions/ipv4/config/dscp-set
    *   qos/classifiers/classifier/terms/term/conditions/ipv6/config/dscp-set
    *   /qos/classifiers/classifier/terms/term/config/id

*   Forwarding Groups

    *   /qos/forwarding-groups/forwarding-group/config/name
    *   /qos/forwarding-groups/forwarding-group/config/output-queue

*   Queue

    *   /qos/queues/queue/config/name

*   Interfaces

    *   /qos/interfaces/interface/input/classifiers/classifier/config/name
    *   /qos/interfaces/interface/output/queues/queue/config/name
    *   /qos/interfaces/interface/output/scheduler-policy/config/name

*   Scheduler policy

    *   /qos/scheduler-policies/scheduler-policy/config/name
    *   /qos/scheduler-policies/scheduler
        -policy/schedulers/scheduler/config/priority
    *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/config/sequence
    *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/config/type
    *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/id
    *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/input-type
    *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/queue
    *   /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/weight

## OpenConfig Path and RPC Coverage

The below yaml defines the OC paths intended to be covered by this test. OC
paths used for test setup are not listed here.

```yaml
paths:
  ## Config paths:
  /qos/forwarding-groups/forwarding-group/config/name:
  /qos/forwarding-groups/forwarding-group/config/output-queue:
  /qos/queues/queue/config/name:
  /qos/classifiers/classifier/config/name:
  /qos/classifiers/classifier/config/type:
  /qos/classifiers/classifier/terms/term/actions/config/target-group:
  /qos/classifiers/classifier/terms/term/conditions/ipv4/config/dscp-set:
  /qos/classifiers/classifier/terms/term/conditions/ipv6/config/dscp-set:
  /qos/classifiers/classifier/terms/term/config/id:
  /qos/interfaces/interface/output/queues/queue/config/name:
  /qos/interfaces/interface/input/classifiers/classifier/config/name:
  /qos/interfaces/interface/output/scheduler-policy/config/name:
  /qos/scheduler-policies/scheduler-policy/config/name:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/config/priority:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/config/sequence:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/config/type:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/id:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/input-type:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/queue:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/config/weight:

  ## State paths:
  /qos/forwarding-groups/forwarding-group/state/name:
  /qos/forwarding-groups/forwarding-group/state/output-queue:
  /qos/queues/queue/state/name:
  /qos/classifiers/classifier/state/name:
  /qos/classifiers/classifier/state/type:
  /qos/classifiers/classifier/terms/term/actions/state/target-group:
  /qos/classifiers/classifier/terms/term/conditions/ipv4/state/dscp-set:
  /qos/classifiers/classifier/terms/term/conditions/ipv6/state/dscp-set:
  /qos/classifiers/classifier/terms/term/state/id:
  /qos/interfaces/interface/output/queues/queue/state/name:
  /qos/interfaces/interface/input/classifiers/classifier/state/name:
  /qos/interfaces/interface/output/scheduler-policy/state/name:
  /qos/scheduler-policies/scheduler-policy/state/name:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/state/priority:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/state/sequence:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/state/type:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/state/id:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/state/input-type:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/state/queue:
  /qos/scheduler-policies/scheduler-policy/schedulers/scheduler/inputs/input/state/weight:

rpcs:
  gnmi:
    gNMI.Set:
      Replace:
```

## Canonical OC
```json
{
  "interfaces": {
    "interface": [
      {
        "config": {
          "description": "Input Interface",
          "name": "port1",
          "type": "ethernetCsmacd"
        },
        "name": "port1"
      },
      {
        "config": {
          "description": "Output Interface",
          "name": "port2",
          "type": "ethernetCsmacd"
        },
        "name": "port2"
      }
    ]
  },
  "qos": {
    "classifiers": {
      "classifier": [
        {
          "config": {
            "name": "qos-policy"
          },
          "name": "qos-policy",
          "terms": {
            "term": [
              {
                "actions": {
                  "config": {
                    "target-group": "fg-BE1"
                  }
                },
                "conditions": {
                  "ipv4": {
                    "config": {
                      "dscp-set": [
                        1,
                        2,
                        3
                      ]
                    }
                  }
                },
                "config": {
                  "id": "term1"
                },
                "id": "term1"
              },
              {
                "actions": {
                  "config": {
                    "target-group": "fg-NC1"
                  }
                },
                "conditions": {
                  "ipv4": {
                    "config": {
                      "dscp-set": [
                        48,
                        49,
                        50,
                        51,
                        52,
                        53,
                        54,
                        55,
                        56,
                        57,
                        58,
                        59
                      ]
                    }
                  }
                },
                "config": {
                  "id": "term2"
                },
                "id": "term2"
              }
            ]
          }
        }
      ]
    },
    "forwarding-groups": {
      "forwarding-group": [
        {
          "config": {
            "name": "fg-BE1",
            "output-queue": "q-BE1"
          },
          "name": "fg-BE1"
        },
        {
          "config": {
            "name": "fg-NC1",
            "output-queue": "q-NC1"
          },
          "name": "fg-NC1"
        }
      ]
    },
    "interfaces": {
      "interface": [
        {
          "config": {
            "interface-id": "eth0"
          },
          "input": {
            "classifiers": {
              "classifier": [
                {
                  "config": {
                    "name": "qos-policy",
                    "type": "IPV4"
                  },
                  "type": "IPV4"
                }
              ]
            }
          },
          "interface-id": "eth0"
        },
        {
          "config": {
            "interface-id": "port2"
          },
          "interface-id": "port2",
          "output": {
            "queues": {
              "queue": [
                {
                  "config": {
                    "name": "q-BE1"
                  },
                  "name": "q-BE1"
                }
              ]
            },
            "scheduler-policy": {
              "config": {
                "name": "scheduler-policy"
              }
            }
          }
        }
      ]
    },
    "queues": {
      "queue": [
        {
          "config": {
            "name": "q-BE1"
          },
          "name": "q-BE1"
        },
        {
          "config": {
            "name": "q-NC1"
          },
          "name": "q-NC1"
        }
      ]
    },
    "scheduler-policies": {
      "scheduler-policy": [
        {
          "config": {
            "name": "scheduler-policy"
          },
          "name": "scheduler-policy",
          "schedulers": {
            "scheduler": [
              {
                "config": {
                  "sequence": 0
                },
                "inputs": {
                  "input": [
                    {
                      "config": {
                        "id": "NC1",
                        "input-type": "QUEUE",
                        "queue": "q-NC1",
                        "weight": "200"
                      },
                      "id": "NC1"
                    }
                  ]
                },
                "sequence": 0
              },
              {
                "config": {
                  "sequence": 1
                },
                "inputs": {
                  "input": [
                    {
                      "config": {
                        "id": "BE1",
                        "input-type": "QUEUE",
                        "queue": "q-BE1",
                        "weight": "1"
                      },
                      "id": "BE1"
                    }
                  ]
                },
                "sequence": 1
              }
            ]
          }
        }
      ]
    }
  }
}
```
