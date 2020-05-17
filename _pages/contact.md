---
title: "Contact"
permalink: "/contact.html"
---


<form action="https://us-central1-abc-dbcfny.cloudfunctions.net/addMessage" method="GET" target="dummyframe">
<p class="mb-4">Say Hello!</p>
<textarea rows="8" class="form-control mb-3" name="text" placeholder="Message*" required style='height:80px'></textarea>
<input class="btn btn-success" type="submit" value="Send">
</form>

<iframe name="dummyframe" id="dummyframe" class='form-control' style='width:100%; height: 40px;margin-top:10px;'></iframe>

<form action="https://formspree.io/{{site.email}}" method="POST">    
<p class="mb-4">Please send and email to {{site.name}}. I will reply as soon as possible!</p>
<div class="form-group row">
<div class="col-md-6">
<input class="form-control" type="text" name="name" placeholder="Name*" required>
</div>
<div class="col-md-6">
<input class="form-control" type="email" name="_replyto" placeholder="E-mail Address*" required>
</div>
</div>
<textarea rows="8" class="form-control mb-3" name="message" placeholder="Message*" required></textarea>    
<input class="btn btn-success" type="submit" value="Send">
</form>
