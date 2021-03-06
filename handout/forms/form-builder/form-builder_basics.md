# FormBuilder Basics

Several steps beyond Angular 2‘s `ngForm` directive is the `FormBuilder`. There is a little bit of black-magic in its simplicity, at first, but after you're comfortable with the basics, learning its building blocks will allow you to handle more complex use cases.

To begin with FormBuilder, we first ensure we are working with the right directives, and the right classes.
We need to load a different set of classes and directives, in order to take advantage of procedural forms.
In our case, these are `FormBuilder`, `FormGroup` and `FormControl`.
We get to take a bit of a shortcut, in that Angular allows us to import a list of all of the helpful directives for us.

In our case, to build a login form, we're looking at something like the following:

_app/login-form.component.ts_
```ts
import {Component} from '@angular/core';
import {
  REACTIVE_FORM_DIRECTIVES,
  FormBuilder,
  FormControl
} from '@angular/forms';

@Component({
  selector: 'login-form',
  templateUrl: 'app/login-form.component.html',
  directives: [REACTIVE_FORM_DIRECTIVES]
})
export class LoginForm {
  loginForm: FormGroup;
  username: FormControl;
  password: FormControl;

  constructor (builder: FormBuilder) {
    this.username = new FormControl('', []);
    this.password = new FormControl('', []);
    this.loginForm = builder.group({
      username: this.username,
      password: this.password
    });
  }

  login () {
    console.log(this.loginForm.value);
    // Attempt Logging in...
  }
}
```

_app/login-form.component.html_
```html
<form [formGroup]="loginForm" (ngSubmit)="login()">
  <label for="username">username</label>
  <input type="text" name="username" id="username" [formControl]="username">

  <label for="password">password</label>
  <input type="password" name="password" id="password" [formControl]="password">

  <button type="submit">log in</button>
</form>
```

[View Example](https://plnkr.co/edit/PiCgcL?p=preview)

### `FormControl`
You will note that the `FormControl` class is assigned to similarly named fields, both on `this` and in the `FormBuilder#group({ })` method.
This is mostly for ease of access.
By saving references to the `FormControl` instances on `this`, you can access the inputs in the template, without having to reference the form, itself.
The form fields can otherwise be reached in the template by using `loginForm.controls.username` and `loginForm.controls.password`.
Likewise, any instance of `FormControl` in this situation, can access its parent group by using its `.root` property (eg: `username.root.controls.password`).
Just be careful to make sure that `root` and `controls` exist, before they're used.

When constructing a `FormControl`, it takes two properties: an initial value, and a list of validators.
Right now, we have no validation. This will be added in the next steps.
