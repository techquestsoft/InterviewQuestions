# 1. Few Spring APIs that use JAXB for serializing POJOs to XMLs.
    In lot of these implementations,the JAXBContext is initialized for each 
    serialization request. The initialization of the JAXBContext is very heavy 
    operation, and there should only be a Single JAXBContext in the application 
    context, initialized at start-up, and shared across threads, it is thread-safe.

    There is 10x performance improvement between a single JAXBContext 
    initialization, and initializing it for every serialization request, so 
    there is potential for major performance gains with this small tweak. 

# 2. One of the Microservice generating token for every transaction that will connect to backend thru REST/SOAP to get data from Backend systems like Mainframe
    This microservice's TPS is 300, so we implemented token generation 
    hourly instead of for every transaction, this improved significant performance.
