FROM public.ecr.aws/sam/build-java21:latest as build-image

WORKDIR "/task"
COPY src/ src/
COPY pom.xml ./

RUN mvn -q clean install
RUN mvn dependency:copy-dependencies -DincludeScope=compile

FROM public.ecr.aws/lambda/java:21

COPY --from=build-image /task/target/classes /var/task/
COPY --from=build-image /task/target/dependency /var/task/lib
# Command can be overwritten by providing a different command in the template directly.
CMD ["authFrameFlow.App::handleRequest"]