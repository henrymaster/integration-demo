# integration-demo

http://www.webxml.com.cn/webservices/qqOnlineWebService.asmx?op=qqCheckOnline

请求
    <qqCheckOnline xmlns="http://WebXml.com.cn/">
      <qqCode>string【参数】</qqCode>
    </qqCheckOnline>
    
响应
    <qqCheckOnlineResponse xmlns="http://WebXml.com.cn/">
      <qqCheckOnlineResult>string</qqCheckOnlineResult>
    </qqCheckOnlineResponse>
    
    public interface QQOnline {

        @Gateway(requestChannel = "check.input")
        string qqCheckOnline(string qqCode);

    }    
    
        public IntegrationFlow check() {
        return f -> f
                .transform(payload ->
                        "<qqCheckOnline xmlns=\"http://WebXml.com.cn/\">"
                                + "<qqCode>" + payload + "</qqCode>"
                                + "</qqCheckOnline>")
                .enrichHeaders(h -> h
                        .header(WebServiceHeaders.SOAP_ACTION,
                                "http://WebXml.com.cn/qqCheckOnline"))
                .handle(new SimpleWebServiceOutboundGateway(
                        "http://www.webxml.com.cn/webservices/qqOnlineWebService.asmx"))
                .transform(new XPathTransformer("/*[local-name()=\"qqCheckOnlineResponse\"]"
                        + "/*[local-name()=\"qqCheckOnlineResult\"]"));
    }
