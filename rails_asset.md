# Asset

## Mailer
* Mass mailing is best done outside of Rails
* mailer: `rails g mailer UserMailer discount finish_transaction`
* example customization `app/mailer/user_mailer.rb`
```
default from: windwalker@trip.com,
        cc: windwalker@trip.com,
        bcc: windwalker@trip.com,
        reply_to: windwalker@trip.com
def finish_transaction(user)
  @user = user
  attachments["receipt.pdf"] = File.read("#{Rails.root}/asset/receipt.pdf")
  mail to: @user.email, subject: "Thank you for using Wind Walker"
end
```
* `app/models/user.rb`
```
private

def  finish_transaction_notification
  UserMailer.finish_transaction(self).deliver
end
```

## Asset Pipeline
* `<%= javascript_include_tag "custom" %>`
```
<script src="/assets/custom-hash.js" type="text/javascript"></script>
```
* `stylesheet_link_tag "style"`
```
<link href="/assets/style-hash.css" media="screen" 
      rel="stylesheet" type="text/css" />
```
* access asset in `assets/stylesheet`
  1. add `.erb` to the css file
  2. embed the path with `<%= asset_path('filename.png') %>`
* Before production: `rake assets:precompile`
* To incldue javascript from `lib/assets/javascripts/shared.js` or `vendor/assets/javascripts/jquery.backstretch.js`, do this in `app/assets/javascripts/application.js`
```
//= require shared
//= require jquery.backstretch
```
