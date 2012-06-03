# Crop and Rotate

Crop and Rotate images in Rils 3+

* [jcrop](http://deepliquid.com/projects/Jcrop/)
* [jqueryrotate](http://code.google.com/p/jqueryrotate)

## Install

In Rails Gemfile

`gem 'crop_rotate'`

## Usage

In application.css manifest

```css
/*
 * require 'jquery.Jcrop.min'
*/
```

In application.js manifest

```javascript
//
//= require 'jquery.color'
//= require 'jquery.Jcrop.min'
//= require 'jquery.Rotate'
//
```

Then use as described in Jquery Rotate and JCrop documentation on respective sites.

## Jquery Rotate

[Jquery Rotate manual](http://code.google.com/p/jqueryrotate/wiki/Documentation)

`$("#img").rotate(90);`

## JCrop

[Jcrop manual](http://deepliquid.com/content/Jcrop_Manual.html)

[jCrop Rails example](http://jsfiddle.net/ambiguous/jTvV3/)

```coffeescript
jQuery ->
  class AvatarCropper
    constructor: -> 
      $('#cropbox').Jcrop 
        aspectRatio: 1 
        setSelect: [0, 0, 500, 500] 
        onSelect: @update 
        onChange: @update

    update: (coords) => 
      $('#user_crop_x').val(coords.x) 
      $('#user_crop_y').val(coords.y) 
      $('#user_crop_w').val(coords.w) 
      $('#user_crop_h').val(coords.h) 
      @updatePreview(coords)

    updatePreview: (coords) => 
      $('#preview').css
        width: Math.round(100/coords.w * $('#cropbox').width()) + 'px' 
        height: Math.round(100/coords.h * $('#cropbox').height()) + 'px'  
        marginLeft: '-' + Math.round(100/coords.w * coords.x) + 'px' 
        marginTop: '-' + Math.round(100/coords.h * coords.y) + 'px'

  new AvatarCropper()
```

```ruby
class UserController < ApplicationController
  def update
      @user = User.find(params[:id])
      if @user.update_attributes(params[:user])
        if params[:user][:avatar].present?
          render :crop
        else
          redirect_to @user, notice: "Successfully updated user."
        end
      else
        render :new
      end
    end

  def crop
    @user = User.find(params[:id])
    render view: "crop"
  end
end
```

```haml
%h1
  Crop Avatar
= image_tag @user.avatar_url(:pre_crop), id: "cropbox"

%h4 
  Preview
= image_tag @user.avatar.url(:pre_crop), id: "preview-box"

= form_for @user do |f|
  .actions
    - %w[x y w h].each do |attribute|
      = f.hidden_field "crop_#{attribute}"
    = f.submit "Crop"
```

```css
#cropbox {
  /* background: url(http://placekitten.com/400/400); */
  width: 400px;
  height: 400px;
}
#preview-box {
  width: 100px;
  height: 100px;
  overflow: hidden;
}
```

## Updates

Please keep me posted on new updates to each library ;)

## Contributing to crop_rotate
 
* Check out the latest master to make sure the feature hasn't been implemented or the bug hasn't been fixed yet.
* Check out the issue tracker to make sure someone already hasn't requested it and/or contributed it.
* Fork the project.
* Start a feature/bugfix branch.
* Commit and push until you are happy with your contribution.
* Make sure to add tests for it. This is important so I don't break it in a future version unintentionally.
* Please try not to mess with the Rakefile, version, or history. If you want to have your own version, or is otherwise necessary, that is fine, but please isolate to its own commit so I can cherry-pick around it.

## Copyright

Copyright (c) 2012 Kristian Mandrup. See LICENSE.txt for
further details.

