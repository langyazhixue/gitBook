# 表单操作管理的实现

## ant Design-form
- form的包装

    该组件的一些API需要经过`Form.create({})(conponentName)`的包装之后才能使用.经过包装的组件
    带有`this.props.form`属性。该属性有很多控制表单行为的方法。如：`getFieldValue`,`getFieldsValue`,`setFieldsValue`等等。
    ps:使用 getFieldsValue getFieldValue setFieldsValue 等时，应确保对应的 field 已经用 getFieldDecorator 注册过了。
    getFieldDecorator方法对表单进行双向绑定。详见：[getFieldDecorator](https://ant.design/components/form-cn/)
    
    ```javascript
    const WrappedHorizontalLoginForm = Form.create()(HorizontalLoginForm);
    ```

- Form.Item

    一个Form.Item中只放一个经过`getFieldDecoration`装饰过的child.该child会被自动添加value,onChange等属性。
    `getFieldDecorator(id,options)`:其中的option一般设置field的校验规则(rules)等参数。
    ```javascript
    <FormItem
      validateStatus={userNameError ? 'error' : ''}
      help={userNameError || ''}
    >
      {getFieldDecorator('userName', {
        rules: [{ required: true, message: 'Please input your username!' }],
      })(
        <Input placeholder="Username" />
      )}
    </FormItem>
    ```
    
- 表单的提交和校验

   响应form的onSubmit事件。`this.props.form.validateFields`将获取一组field的value和error。若校验无误，则提交表单。
  ```javascript
  handleSubmit = (e) => {
      e.preventDefault();
      this.props.form.validateFields((err, values) => {
        if (!err) {
          console.log('Received values of form: ', values);
        }
      });
    }
  ```