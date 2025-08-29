# ----------------------
# Build stage
# ----------------------
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /app

# Copy the project file(s) and restore dependencies
COPY *.csproj ./
RUN dotnet restore

# Copy the rest of the source code
COPY . ./

# Publish the app to the 'out' folder
RUN dotnet publish -c Release -o out

# ----------------------
# Runtime stage
# ----------------------
FROM mcr.microsoft.com/dotnet/runtime:7.0
WORKDIR /app

# Copy the published output from the build stage
COPY --from=build /app/out ./

# Set environment variables (replace with your actual values)
ENV k=your_discord_token_here
ENV h=your_hmac_key_here

# Run the bot
ENTRYPOINT ["dotnet", "SecureAuthBot.dll"]
