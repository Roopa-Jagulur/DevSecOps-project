FROM eclipse-temurin:21-jdk AS builder

WORKDIR /usr/src/app/

COPY gradlew* settings.gradle* build.gradle .
COPY ./gradle ./gradle

RUN chmod +x ./gradlew
RUN ./gradlew
RUN ./gradlew downloadRepos

COPY . .
COPY ./pb ./proto
RUN chmod +x ./gradlew
RUN ./gradlew installDist -PprotoSourceDir=./proto

#####################################################
# Final stage- using light weight image i.e jre
FROM eclipse-temurin:21-jre 

# Create non-root user and group
RUN groupadd -r dsouser && useradd -r -g dsouser dsouser

WORKDIR /usr/src/app/

# Copy files and set ownership
COPY --from=builder /usr/src/app/ ./
RUN chown -R dsouser:dsouser /usr/src/app

ENV AD_PORT=9099

# Switch to non-root user
USER dsouser

ENTRYPOINT ["./build/install/opentelemetry-demo-ad/bin/Ad"]

