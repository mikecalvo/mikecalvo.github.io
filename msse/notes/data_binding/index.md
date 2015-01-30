footer: © Citronella Software Ltd 2015
slidenumbers: true

# Grails Data Binding
## Mike Calvo
### mike@citronellasoftware.com

----

# Data Binding
- Converting HTML data into strongly-typed Grails object
- Grails converts request parameters to the correct Groovy type
- Several approaches:
  - Action Arguments
  - Domain properties assignment
  - Command Objects

---

# Action Arguments
- Controller action parameters are mapped to request parameters by name
- @RequestParameter can be used to provide a custom mapping

---
# Action Argument Examples
```
def addPost(Long userId, String content) {
  def user = User.get(userId)
  user.addComment(content)
  user.save()
}

def findComment(@RequestParameter('form_comment')String comment) {
  return UserComment.findByText(comment)
}
```

---
# Binding To Objects
- HTTP request parameters can be assigned, in bulk, to domain instances
- Parameters must match domain property names

```
def create() {
  def post = new Post(params)
  post.save()
}
```

---
# Excluding/Including Properties
- Controller has bindData method injected
  - Allow specifying exlcuded and included properties
  `bindData(user, params, ['loginId', 'password'])`
- Limiting properties to set
  `user.properties['email', 'fullName'] = params`

---
# Nested Properties
- Grails binding works for properties within nested objects
- Parameter names should include full dot notation to the property
`<g:textField name="profile.fullName" value="user.profile.fullName" />`

---
# Command Objects
- View Model Object
- Properties map to UI fields
- Can have their own constraints used to validate UI
  - Can import the domain constraints
- Good practice to mark with @Validatable to be useful outside of UI
- Participate in injection: services can be injected into them

---
# Command Object Example
```
class UserRegistrationCommand {
  String loginId
  String password
  String passwordConfirm
  String fullName
  String email

  static constraints {
    importFrom Profile
    importFrom User
    passwordConfirm(nullable: false, validator: { confirm, urc ->
      return confirm == urc.password
    })
  }
}
```

---
# Using Command Objects
- Passed as parameters to controller actions
- HTTP request parameters automatically mapped to command properties

---
# Uploading Files
- File data should be mapped to byte array
- GSP should contain an uploadForm and a file input:

```
<g:uploadForm action="upload">
  Photo: <input name="photo" type="file" />
  <g:submitButton name="upload" value="Upload" />
</g>
```

---
# Receiving File in Action
- File can be mapped to a domain property or saved to disk

```
class PhotoUploadCommand {
  byte[] photo
  String loginId
}
def upload() {
  User.findByLoginId(puc.loginId)
  user.photo = puc.photo

  // OR
  def file = request.getFile('photo')
  file.transferTo(new File(basePhotoDir+'/'+params.loginId))
}
```
