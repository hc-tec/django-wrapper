

```py
from response import Error, Success, validData

class UserLogin(APIView):
    '''
      第一个参数, error_config, 错误配置；
      第二个参数, success_config, 成功配置；
      第三个参数, valid, 是否需要判断接收的参数存在字段为空, 默认为 True;
    '''

    @validData(Error(400, 'Login Failed'), Success(200, 'Login Success'))
    def post(self, request, *args, **kwargs):

        user_name = request.data.get('user_name')
        password = request.data.get('password')
        password = md5(password)
        User = models.User.objects.filter(user_name=user_name, password=password).first()
        assert User is not None, '此用户不存在'
        ser = serializer.UserInfoSerializer(instance=User, many=False)
        # 只需返回序列化之后或者自定义的数据即可
        return ser.data
```