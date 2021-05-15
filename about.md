---
layout: page
title: About
permalink: /about/
---
<!--- https://codepen.io/erikapdx/pen/BnfjH --->

This is the base Jekyll theme. You can find out more info about customizing your Jekyll theme, as well as basic Jekyll usage documentation at [jekyllrb.com](https://jekyllrb.com/)
<style>

form {
  width: 450px;
  margin: 17% auto;
}

.input {
  color: #FDFCFB;
  display: flex;
  align-items: center;
}

.button {
  height: 42px;
  border: none;
}

#email {
  width: 60%;
  font-family: inherit;
  letter-spacing: 1px;
  text-indent: 5%;
  border-radius: 5px 0 0 5px;
  outline: 2px solid #737373;
  }

#submit {
  width: 25%;
  height: 46px;
  background: #E86C8D;
  font-family: inherit;
  font-weight: bold;
  color: inherit;
  letter-spacing: 1px;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
  transition: background .3s ease-in-out;
}

#submit:hover {
  background: #d45d7d;
}

input:focus {
  outline: none;
  box-shadow: 0 0 2px #E86C8D;
}
</style>

  <form action="#" method="post" name="sign up for beta form" data-netlify="true">
      <div class="input">
        <input type="text" class="button" id="email" name="email" placeholder="NAME@EXAMPLE.COM">
        <input type="submit" class="button" id="submit" value="Subscribe">
      </div>
    </form>
