# Cordys 获取登录用户信息

## 一、应用描述

这是一个用于获取 Cordys 登录用户信息的 MaxKB 智能体应用，可以确认登录用户的角色权限，便于后续涉及角色权限的各种操作。

## 二、应用功能

该应用的功能是获取当前登录用户的详细信息，包括用户名、地区、手机号、邮箱、用户角色。

## 三、应用构建要素

Cordys 获取登录用户信息应用构建时涉及的核心要素内容：
- 工具：Cordys CRM MCP: https://cordys.cn/docs/mcp_server/#41-maxkb
- 获取登陆用户信息工具：工具内容示例：

```python
import requests
import json


def get_user_region_info(user_data):
    """
    获取用户区域信息

    Args:
        user_data: 用户数据字典

    Returns:
        dict: 包含用户信息的JSON
    """
    # 定义区域部门映射
    regions_mapping = {
        "华南区": {
            "销售五部", "销售六部"
        },
        "华东区": {
            "销售一部", "销售二部"
        },
        "华北区": {
            "销售三部", "销售四部"
        },
        "华中区": {
            "销售七部", "销售八部"
        }
    }

    data = user_data.get('data', {})
    department_name = data.get('departmentName', '')

    # 确定区域
    region = "未知"
    for reg, departments in regions_mapping.items():
        if department_name in departments:
            region = reg
            break

    # 构建返回结果
    result = {
        "userName": data.get('userName', ''),
        "region": region,
        "phone": data.get('phone', ''),
        "email": data.get('email', ''),
        "position": data.get('position', '')
    }

    return result

def simple_get(url, auth_key):
    """
    简化的GET请求函数
    """
    token_value = ""
    if len(auth_key) > 0:
        token_value = str(auth_key[0]).strip('"')
    headers = {"authorization": token_value}
    
    try:
        response = requests.get(
            url=url,
            headers=headers,
            timeout=10
        )
        response.raise_for_status()
        return get_user_region_info(response.json())
    except Exception as e:
        print(f"请求失败: {e}")
        return None
```

- 工作流：部分截图
![工作流截图](https://i-blog.csdnimg.cn/direct/da817cfc07e9463396d1032654fbcb0c.png)

## 四、应用效果
![应用效果](https://i-blog.csdnimg.cn/direct/051d38601ffc47248ddf3c221b233ad4.gif)