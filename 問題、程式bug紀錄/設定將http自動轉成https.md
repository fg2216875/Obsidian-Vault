在Web.config中加入這段，可在進入網頁時，自動將http轉址成https，若不想要自動轉址的話，將這段移除即可
```
<system.webServer>
	<rewrite>
		<rules>
			<rule name="SPCR" stopProcessing="true">
				<match url="(.*)" />
				<conditions>
					<add input="{HTTPS}" pattern="^OFF$" />
				</conditions>
				<action type="Redirect" url="https://{HTTP_HOST}{REQUEST_URI}" />
			</rule>
		</rules>
	</rewrite>
</system.webServer>
```