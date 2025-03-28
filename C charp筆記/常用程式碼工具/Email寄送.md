```csharp
//.net framework
public bool SendEmail(){
	// 設定寄件人、收件人、主旨和內容 
	var fromAddress = new MailAddress("your-email@example.com", "Your Name"); 
	var toAddress = new MailAddress("recipient-email@example.com", "Recipient Name"); 
	const string fromPassword = "your-email-password"; 
	const string subject = "This is a test email"; 
	const string body = "Hello,\n\nThis is a test email"; 
	// 設定 SMTP 伺服器 
	var smtp = new SmtpClient { Host = "smtp.example.com", Port = 587, EnableSsl = true, DeliveryMethod = SmtpDeliveryMethod.Network, UseDefaultCredentials = false, Credentials = new NetworkCredential(fromAddress.Address, fromPassword) }; 
	using (var message = new MailMessage(fromAddress, toAddress) { 
		Subject = subject, Body = body }) { 
		try { 
			smtp.Send(message); Console.WriteLine("Email sent successfully!"); 
			return true;
		} 
		catch (Exception ex) { 
			Console.WriteLine("Exception caught in CreateTestMessage2", ex.ToString()); 
			return false;
		} 
	}
}

```