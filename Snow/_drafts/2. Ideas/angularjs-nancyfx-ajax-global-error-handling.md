---
layout: post
title: Migrating To Sandra.Snow
categories: .Net, AngularJs, Javascript, NancyFx
published: draft
---

app.config(function ($httpProvider) {
    $httpProvider.interceptors.push(function ($q, $injector) {
        return {
            'request': function (config) {
                return config || $q.when(config);
            },
            'requestError': function (rejection) {
                return $q.reject(rejection);
            },
            'response': function (response) {
                return response || $q.when(response);
            },
            'responseError': function (response) {
                var dialog = $injector.get('$dialog');
                var status = response.status;
                if (status == 500) {
                    dialog.dialog({
                        dialogFade: true,
                        backdrop: true,
                        resolve: {
                            data: function () {
                                return {
                                    message: response.data.ErrorMessage,
                                    stacktrace: response.data.FullException
                                };
                            }
                        }
                    }).open('/Views/Error.html', 'ErrorCtrl');
                }
                return $q.reject(response);
            }
        };
    });
});
 
 
  protected override void ApplicationStartup(TinyIoCContainer container, IPipelines pipelines)  
        {  
            base.ApplicationStartup(container, pipelines);  
   
            pipelines.OnError += (ctx, ex) =>  
            {  
                _logger.Error("", ex);  
                return ErrorResponse.FromException(ex); ;  
            };  
 
 
public class ErrorResponse : JsonResponse  
    {  
        readonly Error _error;  
   
        private ErrorResponse(Error error)  
            : base(error, new DefaultJsonSerializer())  
        {  
            if (error == null)  
            { throw new ArgumentNullException(); }  
   
            _error = error;  
        }  
   
        public string ErrorMessage { get { return _error.ErrorMessage; } }  
        public string FullException { get { return _error.FullException; } }  
        public string[] Errors { get { return _error.Errors; } }  
   
        public static ErrorResponse FromMessage(string message)  
        {  
            return new ErrorResponse(new Error { ErrorMessage = message });  
        }  
   
        public static ErrorResponse FromException(Exception ex)  
        {  
            var exception = ex.GetRootException();  
            var summary = exception.Message;  
            var statusCode = HttpStatusCode.InternalServerError;  
   
            var error = new Error { ErrorMessage = summary, FullException = exception.ToString() };  
   
            var response = new ErrorResponse(error);  
            response.StatusCode = statusCode;  
            return response;  
        }  
   
        class Error  
        {  
            public string ErrorMessage { get; set; }  
   
            [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]  
            public string FullException { get; set; }  
   
            [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]  
            public string[] Errors { get; set; }  
        }  
    }  