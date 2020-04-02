# Start from standard Ruby build
# The version here must match the version declared in the Gemfile
FROM ruby:2.6.3

# Install Rails dependencies
RUN apt-get update && apt-get install -y \
  build-essential \
  nodejs

# Create working directory in container (does not effect host machine)
RUN mkdir -p /app
WORKDIR /app

# Copy dependencies and Rails application to container's working directory
COPY Gemfile Gemfile.lock ./
RUN gem install bundler && bundle install --jobs 20 --retry 5
COPY . ./

#Expose container port
EXPOSE 3000

# Run server
CMD ["bundle", "exec", "rails", "server", "-b", "0.0.0.0"]