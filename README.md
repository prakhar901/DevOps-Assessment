# DevOps-Assessment
# Use the official Ruby image as base
FROM ruby:latest

# Set environment variables
ENV RAILS_ENV=production \
    RAILS_LOG_TO_STDOUT=true

# Install dependencies
RUN apt-get update -qq && \
    apt-get install -y nodejs postgresql-client && \
    gem install bundler

# Set working directory
WORKDIR /app

# Copy Gemfile and Gemfile.lock
COPY Gemfile Gemfile.lock ./

# Install gems
RUN bundle install --jobs 20 --retry 5 --without development test

# Copy application code
COPY . .

# Expose port 3000 to the Docker host
EXPOSE 3000

# Run database migrations
RUN bin/rails db:create db:migrate

# Start the Rails server
CMD ["bundle", "exec", "rails", "server", "-b", "0.0.0.0"]
