# **official version:**  
  just follow the link below:  
  https://docs.archer2.ac.uk/quick-start/quickstart-users/#request-an-account-on-archer

***

# **personal version:**
  1. Get a SAFE account on the website below, by your UoB email adress:
      https://safe.epcc.ed.ac.uk/login.jsp
  2. Log in, then click on the "login accounts" tab at the top of the page.
  3. "Select correct project group" select: n02 - NCAS (National Centre for Atmospheric Science) from the drop down list.
  4. "Select a machine for the login account" select: archer2
  5. create the public SSH key on bluecrystal and/or BRIDGE server type:
        ssh-keygen -t rsa -f ~/.ssh/id_rsa_puma2
     then upload the generated id_rsa_puma2.pub on the required SAFE page, and click request in the bottom of this page.
  6. your PI will be asked to approve this request. after it approve it, you can proceed to the next step
