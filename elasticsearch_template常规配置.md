## Elasticsearch Template常规配置

####  template配置

参数说明


示例配置如下:


	"_default_": {
		"dynamic_templates": [
		{
			"message_field": {
			"mapping": {
				"index": "analyzed",
				"omit_norms": true,
				"type": "string"
			},
			"match_mapping_type": "string",
			"match": "message"
			}
		},
		{
			"message_field": {
			"mapping": {
				"index": "analyzed",
				"omit_norms": true,
				"type": "string"
			},
			"match_mapping_type": "string",
			"match": "error_detail"
			}
		},
		{
			"string_fields": {
			"mapping": {
				"ignore_above": 256,
				"index": "not_analyzed",
				"omit_norms": true,
				"type": "string",
				"doc_values": true
			},
			"match_mapping_type": "string",
			"match": "*"
			}
		}
		],
		"_all": {
		"omit_norms": true,
		"enabled": true
		},
		"properties": {
		"geoip": {
			"dynamic": "true",
			"properties": {
			"location": {
				"type": "geo_point"
			}
			}
		},
		"@version": {
			"index": "not_analyzed",
			"type": "string"
		}
		}
	},
	"nginx": {
		"dynamic_templates": [
		{
			"message_field": {
			"mapping": {
				"index": "analyzed",
				"omit_norms": true,
				"type": "string"
			},
			"match_mapping_type": "string",
			"match": "error_detail"
			}
		},
		{
			"message_field": {
			"mapping": {
				"index": "analyzed",
				"omit_norms": true,
				"type": "string"
			},
			"match_mapping_type": "string",
			"match": "error_detail"
			}
		},
		{
			"string_fields": {
			"mapping": {
				"ignore_above": 256,
				"index": "not_analyzed",
				"omit_norms": true,
				"type": "string",
				"doc_values": true
			},
			"match_mapping_type": "string",
			"match": "*"
			}
		}
		],
		"_all": {
		"omit_norms": true,
		"enabled": true
		},
		"properties": {
		"error_detail": {
			"norms": {
			"enabled": false
			},
			"type": "string"
		},
		"type": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"mac": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"platform": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"network": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"hour": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"system_version": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"@version": {
			"index": "not_analyzed",
			"type": "string"
		},
		"user_akcount": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"model": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"state": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"apk_version": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"system_versiof": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"error_time": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"geoip": {
			"dynamic": "true",
			"properties": {
			"isp_cn": {
				"ignore_above": 256,
				"index": "not_analyzed",
				"type": "string",
				"doc_values": true
			},
			"province_cn": {
				"ignore_above": 256,
				"index": "not_analyzed",
				"type": "string",
				"doc_values": true
			},
			"location": {
				"type": "geo_point"
			},
			"country_cn": {
				"ignore_above": 256,
				"index": "not_analyzed",
				"type": "string",
				"doc_values": true
			}
			}
		},
		"manufacturers": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"os": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"ip": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"tags": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"@timestamp": {
			"format": "dateOptionalTime",
			"type": "date"
		},
		"user_id": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"data_type": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"error_code": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"user_account": {
			"ignore_above": 256,
			"index": "not_analyzed",
			"type": "string",
			"doc_values": true
		},
		"time": {
			"type": "long"
		}
		}
	}
	}