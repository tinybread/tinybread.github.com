---
layout: post
category: Spring
title: SessionAttributes
tagline: by TinyBread
tags: [Spring]
---


<!--more-->

  
# @SessionAttributes & SessionStatus

### Session?   

브라우저를 통해 사용자가 로그인을 하면 그 정보는 로그아웃이 되기 전까지 유지되어야 한다. 그러나 HTTP 요청은 매 요청과 응답이 독립적으로 이루어지기 때문에 사용자의 로그인 정보를 유지하기 위해 Session을 사용한다.


### 사용방법       

@SessionAttributes 사용하지 않고 servlet & jsp 에서 session을 사용하는 방법으로는 request에서 Session객체를 얻어 아래와 같은 방식으로 사용한다.<br>

		// get session attribute
		HttpSession session = request.getSession();
		String sessionScope = (String) session.getAttribute("sessionTest");


		// set session attribute
		HttpSession session = request.getSession();
		session.setAttribute("sessionTest", "session Test!!");


<br><br>
**@SessionAttributes, @ModelAttribute** 사용하게되면 위와 같이 직접 세션에 저장을 해주는 작업을 하지않아도 된다.
<img src="/assets/themes/Snail/img/Spring/SessionAttributes/sampleNoticeLogin.PNG" alt="">
<br>  

		@SessionAttributes("user")  
		...
		...
		model.addAttribute("user", user);
		...

위와 같은 작업을 통해 "user"라는 이름으로 Model객체에 attribute를 지정하는 것이 있다면 이를 Session에 저장한다.<br>  

			@RequestMapping(value = "/sessionCheck", method = RequestMethod.POST)
			public String submit(@ModelAttribute("user") User user) {
				System.out.println("id : " + user.getId());
				System.out.println("pw : " + user.getPasswd());
				return "result";
			}

@ModelAttribute를 이용하여 session에 저장된 값을 파라미터값으로 가져온다. (session에 저장된 값을 꺼내옴)


<br>  
### SessionStatus  

더이상 필요없는 세션을 제거해 주기 위한 작업 (SessionStatus스프링 내장 타입)

			@RequestMapping(value = "/logout", method = RequestMethod.POST)
			public String logout(@ModelAttribute User user, SessionStatus sessionStatus) {
				sessionStatus.setComplete();//session 제거  
				return "result";
			}





<br>  

